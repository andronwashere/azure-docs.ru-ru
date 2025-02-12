---
title: Устранение неполадок для расширения NPS для многофакторной проверки Подлинности Azure — Azure Active Directory
description: Узнайте, как решить проблемы, связанные с расширением NPS для Многофакторной идентификации Azure
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 11/13/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: c0d04db6e9ccedc1e67ed0cdfd914ab42ebea0b1
ms.sourcegitcommit: 084630bb22ae4cf037794923a1ef602d84831c57
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2019
ms.locfileid: "67536939"
---
# <a name="resolve-error-messages-from-the-nps-extension-for-azure-multi-factor-authentication"></a>Устранение ошибок, связанных с расширением NPS для Многофакторной идентификации Azure.

В этой статье приведены способы быстрого устранения ошибок, связанных с расширением NPS для Многофакторной идентификации Azure. Журналы расширения NPS можно найти в средстве просмотра событий в разделе **Custom Views** > **Server Roles** > **Network Policy and Access Services** (Настраиваемые представления > Роли сервера > Службы доступа и политики сети) на сервере, на котором установлено расширение NPS.

## <a name="troubleshooting-steps-for-common-errors"></a>Действия по устранению распространенных ошибок

| Код ошибки | Действия по устранению неполадок |
| ---------- | --------------------- |
| **CONTACT_SUPPORT** | [Обратитесь в службу поддержки](#contact-microsoft-support) и укажите, как вы собирали журналы. Предоставьте как можно больше сведений о действиях, которые были выполнены до возникновения ошибки, включая идентификатор клиента и имя участника-пользователя (UPN). |
| **CLIENT_CERT_INSTALL_ERROR** | Возможно, проблема заключается в способе установки клиентского сертификата или его связи с клиентом. Следуйте инструкциям, описанным в разделе по [устранению неполадок](howto-mfa-nps-extension.md#troubleshooting), чтобы определить проблемы с сертификатом клиента. |
| **ESTS_TOKEN_ERROR** | Следуйте инструкциям, описанным в разделе по [устранению неполадок](howto-mfa-nps-extension.md#troubleshooting), чтобы определить проблемы с сертификатом клиента и маркером ADAL. |
| **HTTPS_COMMUNICATION_ERROR** | NPS-сервер не получает ответы из Azure MFA. Проверьте, открыты ли брандмауэры для входящего и исходящего трафика https://adnotifications.windowsazure.com. |
| **HTTP_CONNECT_ERROR** | Убедитесь, что у сервера, на котором запущено расширение NPS, есть доступ к https://adnotifications.windowsazure.com и https://login.microsoftonline.com/. Если эти сайты не загружаются, необходимо устранить неполадки подключения на сервере. |
| **Расширение NPS для Azure MFA** <br> Расширение NPS для Azure MFA выполняет только дополнительную аутентификацию для запросов Radius с состоянием AccessAccept. Запрос, полученный для имени пользователя с состоянием ответа AccessReject, игнорируется. | Эта ошибка обычно означает сбой аутентификации в AD или то, что серверу NPS не удается получить ответы от Azure AD. Убедитесь, что используемые брандмауэры открыты в обоих направлениях для входящего и исходящего трафика с адресов https://adnotifications.windowsazure.com и https://login.microsoftonline.com на портах 80 и 443. Также необходимо убедиться, что на вкладке DIAL-IN (Входящие подключения) раздела Network Access Permissions (Разрешения на доступ к сети) установлено значение параметра control access through NPS Network Policy. Эта ошибка также может активировать, если пользователю не назначена лицензия. |
| **REGISTRY_CONFIG_ERROR** | В реестре для приложения отсутствует раздел. Такое может случиться, если после установки не был запущен [сценарий PowerShell](howto-mfa-nps-extension.md#install-the-nps-extension). В сообщении об ошибке должен быть указан отсутствующий раздел. Убедитесь, что этот раздел находится в HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\AzureMfa. |
| **REQUEST_FORMAT_ERROR** <br> Отсутствует обязательный атрибут userName\Identifier для запроса RADIUS. Убедитесь, что NPS-сервер получает запросы RADIUS. | В этом случае проблема связана с установкой. Расширение NPS должно быть установлено на серверах NPS, которые получают запросы RADIUS. NPS-серверы, установленные как зависимости для таких служб, как RDG и RRAS, запросы RADIUS не получают. В случае такой установки расширение NPS не будет работать, а ошибки будут возникать из-за невозможности прочитать сведения из запроса проверки подлинности. |
| **REQUEST_MISSING_CODE** | Убедитесь, что протокол шифрования паролей между серверами NAS и NPS поддерживает дополнительный метод аутентификации, который вы используете. **PAP** поддерживает все методы аутентификации Azure MFA в облаке: телефонный звонок, одностороннее текстовое сообщение, уведомление мобильного приложения и код проверки мобильного приложения. **CHAPV2** и **EAP** поддерживают телефонный звонок или уведомление мобильного приложения. |
| **USERNAME_CANONICALIZATION_ERROR** | В локальном экземпляре Active Directory должен присутствовать пользователь, а у службы NPS должны быть разрешения на доступ к каталогу. Если вы используете отношения доверия между лесами, [обратитесь в службу поддержки](#contact-microsoft-support) для получения дополнительных сведений. |

### <a name="alternate-login-id-errors"></a>Ошибки с альтернативным именем пользователя

| Код ошибки | Сообщение об ошибке | Действия по устранению неполадок |
| ---------- | ------------- | --------------------- |
| **ALTERNATE_LOGIN_ID_ERROR** | Ошибка. Сбой поиска userObjectSid. | Убедитесь, что такой пользователь существует в экземпляре локальной службы Active Directory. Если вы используете отношения доверия между лесами, [обратитесь в службу поддержки](#contact-microsoft-support) для получения дополнительных сведений. |
| **ALTERNATE_LOGIN_ID_ERROR** | Ошибка: сбой поиска альтернативного имени пользователя. | Убедитесь, что для значения реестра LDAP_ALTERNATE_LOGINID_ATTRIBUTE указан [допустимый атрибут Active Directory](https://msdn.microsoft.com/library/ms675090(v=vs.85).aspx). <br><br> Если для LDAP_FORCE_GLOBAL_CATALOG задано значение True, а LDAP_LOOKUP_FORESTS имеет непустое значение, проверьте, настроен ли глобальный каталог и добавлен ли к нему атрибут AlternateLoginId. <br><br> Если LDAP_LOOKUP_FORESTS имеет непустое значение, убедитесь, что это значение правильно. Если указано несколько имен леса, такие имена должны быть разделены точками с запятой без пробелов. <br><br> Если с помощью этих шагов не удалось устранить проблему, обратитесь в [службу поддержки](#contact-microsoft-support) для получения дополнительных сведений. |
| **ALTERNATE_LOGIN_ID_ERROR** | Ошибка: пустое значение альтернативного имени пользователя. | Убедитесь, что атрибут AlternateLoginId настроен для пользователя. |

## <a name="errors-your-users-may-encounter"></a>Ошибки, с которыми могут столкнуться пользователи

| Код ошибки | Сообщение об ошибке | Действия по устранению неполадок |
| ---------- | ------------- | --------------------- |
| **AccessDenied** | У вызывающего клиента нет разрешений на выполнение проверки подлинности пользователя. | Домен клиента и домен имени участника-пользователя (UPN) должны совпадать. Например, user@contoso.com должен проходить проверку подлинности для клиента Contoso. Имя участника-пользователя представляет допустимое имя пользователя для клиента в Azure. |
| **AuthenticationMethodNotConfigured** | Указанный метод проверки подлинности не настроен для пользователя. | Пользователь должен добавить метод или узнать, были ли настроены методы проверки согласно инструкциям, описанным в статье [Управление параметрами двухфакторной проверки подлинности](../user-help/multi-factor-authentication-end-user-manage-settings.md). |
| **AuthenticationMethodNotSupported** | Указанный метод проверки подлинности не поддерживается. | Соберите все журналы, которые содержат эту ошибку, и [обратитесь в службу поддержки](#contact-microsoft-support). Укажите имя пользователя и дополнительный метод проверки подлинности, который вызвал ошибку. |
| **BecAccessDenied** | Вызов MSODS Bec вернул ошибку "Доступ запрещен". Скорее всего, в клиенте не определено имя пользователя. | Пользователь присутствует в локальной службе Active Directory, но не синхронизирован в Azure AD с помощью AD Connect. Или же для клиента пользователь отсутствует. Добавьте пользователей в Azure AD, а они должны добавить методы проверки согласно инструкциям, описанным в статье [Управление параметрами двухфакторной проверки подлинности](../user-help/multi-factor-authentication-end-user-manage-settings.md). |
| **InvalidFormat** или **StrongAuthenticationServiceInvalidParameter** | Номер телефона указан в неизвестном формате. | Пользователь должен исправить номер телефона, используемый для проверки. |
| **InvalidSession** | Указанный сеанс является недопустимым, или же истек срок его действия. | Для завершения сеанса потребовалось более 3 минут. В течение 3 минут после инициирования запроса проверки подлинности пользователь должен ввести код проверки или же ответить на уведомление от приложения. Если после этого проблема не устранена, убедитесь, что нет сетевых задержек между клиентом, серверами NAS, NPS и конечной точкой Azure MFA.  |
| **NoDefaultAuthenticationMethodIsConfigured** | Для пользователя не настроен метод проверки подлинности по умолчанию. | Пользователь должен добавить метод или узнать, были ли настроены методы проверки согласно инструкциям, описанным в статье [Управление параметрами двухфакторной проверки подлинности](../user-help/multi-factor-authentication-end-user-manage-settings.md). Пользователь должен выбрать метод проверки подлинности по умолчанию и настроить его для своей учетной записи. |
| **OathCodePinIncorrect** | Введен неправильный пароль и ПИН-код. | Это непредвиденная ошибка в расширении NPS. В случае возникновения этой ошибки [обратитесь в службу поддержки](#contact-microsoft-support) для получения сведений по устранению проблемы. |
| **ProofDataNotFound** | Для указанного метода проверки подлинности не настроены данные подтверждения. | Пользователь должен использовать другой метод проверки или же добавить новые методы проверки в соответствии с инструкциями, описанными в статье [Управление параметрами двухфакторной проверки подлинности](../user-help/multi-factor-authentication-end-user-manage-settings.md). Если после подтверждения правильной настройки метода проверки ошибка не устранена, [обратитесь в службу поддержки](#contact-microsoft-support). |
| **SMSAuthFailedWrongCodePinEntered** | Введен неправильный пароль и ПИН-код. (OneWaySMS) | Это непредвиденная ошибка в расширении NPS. В случае возникновения этой ошибки [обратитесь в службу поддержки](#contact-microsoft-support) для получения сведений по устранению проблемы. |
| **TenantIsBlocked** | Клиент заблокирован. | [Обратитесь в службу поддержки](#contact-microsoft-support), указав идентификатор каталога со страницы свойств Azure AD на портале Azure. |
| **UserNotFound** | Указанный пользователь не найден. | Клиент больше не отображается как активный в Azure AD. Убедитесь, что подписка активна и у пользователя есть собственные необходимые приложения. Кроме этого, клиент в субъекте сертификата должен работать правильно, а сертификат должен быть допустимым и зарегистрированным в субъекте-службе. |

## <a name="messages-your-users-may-encounter-that-arent-errors"></a>Другие сообщения, с которыми могут столкнуться пользователи

В некоторых случаях пользователи могут получить сообщения от службы Многофакторной идентификации из-за сбоя запроса проверки подлинности. Эти не ошибки конфигурации продукта, а предупреждения, которые объясняют причину сбоя запроса проверки подлинности.

| Код ошибки | Сообщение об ошибке | Рекомендуемые действия | 
| ---------- | ------------- | ----------------- |
| **OathCodeIncorrect** | Введен неправильный код. Неправильный OATH-код. | Пользователь ввел неправильный код. Пользователь должен повторить попытку ввода. | 
| **SMSAuthFailedMaxAllowedCodeRetryReached** | Достигнуто максимальное число попыток ввода кода. | Слишком много неудачных попыток ввода кода. В зависимости от параметров может потребоваться помощь администратора.  |
| **SMSAuthFailedWrongCodeEntered** | Введен неправильный код. SMS: неверный OTP. | Пользователь ввел неправильный код. Пользователь должен повторить попытку ввода. |

## <a name="errors-that-require-support"></a>Ошибки, при возникновении которых стоит обратиться в службу поддержки

Если вы столкнулись с ошибками, приведенными ниже, мы советуем [обратиться в службу поддержки](#contact-microsoft-support) для помощи в диагностике. Это нестандартные ошибки, поэтому они требуют отдельного анализа. При обращении в службу поддержки не забудьте предоставить как можно больше сведений о действиях, которые привели к возникновению ошибки, а также сведений о клиенте.

| Код ошибки | Сообщение об ошибке |
| ---------- | ------------- |
| **InvalidParameter** | Запрос не может иметь значение null. |
| **InvalidParameter** | Параметр ObjectId не может иметь значение null или быть пустым для ReplicationScope:{0} |
| **InvalidParameter** | Длина CompanyName \{0}\ не должна превышать максимально допустимое значение {1} |
| **InvalidParameter** | Параметр UserPrincipalName не может иметь значение null или быть пустым. |
| **InvalidParameter** | Идентификатор клиента указан в неправильном формате. |
| **InvalidParameter** | Параметр SessionId не может иметь значение null или быть пустым. |
| **InvalidParameter** | Не удалось распознать данные для подтверждения из запроса или MSODS. Данные для подтверждения не распознаны. |
| **InternalError** |  |
| **OathCodePinIncorrect** |  |
| **VersionNotSupported** |  |
| **MFAPinNotSetup** |  |

## <a name="next-steps"></a>Дальнейшие действия

### <a name="troubleshoot-user-accounts"></a>Устранение неполадок, связанных с учетными данными пользователей

Если пользователи столкнулись с проблемами [двухфакторной проверки подлинности](../user-help/multi-factor-authentication-end-user-troubleshoot.md), они могут выполнить самостоятельную диагностику.

### <a name="contact-microsoft-support"></a>Обратиться в службу поддержки Майкрософт

Если вам требуется дополнительная помощь, обратитесь к службе поддержки, используя [поддержку сервера Многофакторной идентификации Azure](https://support.microsoft.com/oas/default.aspx?prid=14947). Вы можете облегчить нам задачу, если при обращении предоставите максимально полные сведения о проблеме. Вы можете сообщить нам, на какой странице возникла ошибка, а также точный код ошибки, идентификатор сеанса и идентификатор пользователя, получившего эту ошибку, и журналы отладки.

Чтобы собрать журналы отладки для помощи в диагностике, выполните следующие инструкции на сервере NPS:

1. Откройте редактор реестра и перейдите по пути HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\AzureMfa. Установите для **VERBOSE_LOG** значение **TRUE**.
2. Откройте командную строку администратора и выполните следующие команды:

   ```
   Mkdir c:\NPS
   Cd NPS
   netsh trace start Scenario=NetConnection capture=yes tracefile=c:\NPS\nettrace.etl
   logman create trace "NPSExtension" -ow -o c:\NPS\NPSExtension.etl -p {7237ED00-E119-430B-AB0F-C63360C8EE81} 0xffffffffffffffff 0xff -nb 16 16 -bs 1024 -mode Circular -f bincirc -max 4096 -ets
   logman update trace "NPSExtension" -p {EC2E6D3A-C958-4C76-8EA4-0262520886FF} 0xffffffffffffffff 0xff -ets
   ```

3. Воспроизведите проблему.

4. Остановите трассировку с помощью следующих команд:

   ```
   logman stop "NPSExtension" -ets
   netsh trace stop
   wevtutil epl AuthNOptCh C:\NPS\%computername%_AuthNOptCh.evtx
   wevtutil epl AuthZOptCh C:\NPS\%computername%_AuthZOptCh.evtx
   wevtutil epl AuthZAdminCh C:\NPS\%computername%_AuthZAdminCh.evtx
   Start .
   ```

5. Откройте редактор реестра и перейдите по пути HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\AzureMfa. Установите для **VERBOSE_LOG** значение **FALSE**.
6. Выполните архивацию содержимого папки C:\NPS и прикрепите ZIP-файл к запросу в службу поддержки.
