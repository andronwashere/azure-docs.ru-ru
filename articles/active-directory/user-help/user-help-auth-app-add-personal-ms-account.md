---
title: Добавление личных учетных записей, предоставленных корпорацией Майкрософт, в приложение Microsoft Authenticator (Azure Active Directory) | Документация Майкрософт
description: Сведения о добавлении личных учетных записей, предоставленных корпорацией Майкрософт, например Outlook.com или Xbox LIVE, в приложение Microsoft Authenticator для двухфакторной проверки подлинности.
services: active-directory
author: eross-msft
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: user-help
ms.topic: conceptual
ms.date: 01/24/2019
ms.author: lizross
ms.reviewer: olhaun
ms.collection: M365-identity-device-management
ms.openlocfilehash: 502407bbf1cdb47fc8105ed0220babcd9f292faa
ms.sourcegitcommit: 04ec7b5fa7a92a4eb72fca6c6cb617be35d30d0c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/22/2019
ms.locfileid: "68382690"
---
# <a name="add-your-personal-microsoft-accounts"></a>Добавление личных учетных записей Майкрософт

Добавьте личные учетные записи Майкрософт, например Outlook.com и Xbox LIVE, в приложение Microsoft Authenticator как для стандартного двухфакторного процесса проверки подлинности, так и для входа в систему без пароля.

- **Метод стандартной двухфакторной проверки подлинности.** Введите имя пользователя и пароль в устройство, в которое вы выполняете вход, а затем выберите, будет ли приложение Microsoft Authenticator отправлять уведомление или хотите ли вы копировать связанный код проверки на экране **Учетные записи** в приложении Microsoft Authenticator.

- **Метод входа без пароля.** На устройстве, в систему которого нужно войти, введите имя пользователя для личной учетной записи Майкрософт, а затем на мобильном устройстве подтвердите, что это вы, используя отпечаток пальца, распознавание лица или ПИН-код. Для этого метода не нужно вводить пароль.

>[!Important]
>Но для добавления такой учетной записи необходимо скачать и установить приложение Microsoft Authenticator. Если вы еще не сделали этого, выполните шаги, описанные в статье [Начало работы с приложением Microsoft Authenticator](user-help-auth-app-download-install.md).

## <a name="add-your-personal-microsoft-account"></a>Добавление личных учетных записей Майкрософт

Вы можете добавить личную учетную запись Майкрософт, включив двухфакторную проверку подлинности и добавив учетную запись в приложение.

>[!Note]
>Если вы планируете использовать вход в систему без пароля только для своей личной учетной записи Майкрософт, вам не нужно включать двухфакторную проверку подлинности. Тем не менее для обеспечения дополнительной безопасности учетной записи рекомендуется включить двухфакторную проверку.

### <a name="turn-on-two-factor-verification"></a>Включение двухфакторной проверки подлинности

1. На компьютере перейдите на страницу [Сведения о безопасности](https://account.microsoft.com/security) и войдите с помощью личной учетной записи Майкрософт. Например, alain@outlook.com.

2. В нижней части страницы **Сведения о безопасности** выберите ссылку **дополнительные параметры безопасности**.

    ![Страница сведений о безопасности с выделенной ссылкой "Дополнительные параметры безопасности"](./media/user-help-auth-app-add-personal-ms-account/more-security-options-link.png)

3. Перейдите в раздел **Двухфакторная проверка подлинности** и выберите **Вкл.** функцию. Вы также можете отключить ее здесь, если вы больше не хотите использовать ее со своей личной учетной записью.

### <a name="add-your-microsoft-account-to-the-app"></a>Добавление учетной записи Майкрософт

1. Откройте приложение Microsoft Authenticator на своем мобильном устройстве.

2. Выберите **Добавить учетную запись** из значка**Customize and control** (Настройка и управление) в правом верхнем углу.

    ![Страница учетных записей с выделенным значком "Customize and control" (Настройка и управление)](./media/user-help-auth-app-add-personal-ms-account/customize-and-control-icon.png)

3. На странице **Добавить учетную запись** выберите **Личная учетная запись**.

4. Войдите в личную учетную запись с помощью соответствующего адреса электронной почты (например, alain@outlook.com), а затем выберите **Далее**.

    >[!Note]
    >Если у вас нет учетной записи Майкрософт, вы можете создать ее здесь.

5. Введите пароль и затем выберите **Войти**.

    Ваша личная учетная запись добавлена в приложение Microsoft Authenticator.

## <a name="next-steps"></a>Следующие шаги

- Добавив учетные записи в приложение, вы можете использовать их для входа с помощью приложения Authenticator на мобильном устройстве. Дополнительные сведения см. в статье [Вход с использованием телефона вместо ввода пароля](user-help-auth-app-sign-in.md).

- Если у вас возникли проблемы с получением кода проверки для личного учетная запись Майкрософт, см. раздел **Устранение неполадок с кодом проверки** в [учетная запись Майкрософт сведения о безопасности & коды проверки](https://support.microsoft.com/en-us/help/12428/microsoft-account-security-info-verification-codes) .

- Для устройств под управлением iOS вы можете также выполнить резервное копирование в облако учетных данных и связанных с ними параметров приложений (например, порядок учетных записей). Дополнительные сведения см. в статье [Резервное копирование и восстановление учетных данных с помощью приложения Microsoft Authenticator](user-help-auth-app-backup-recovery.md).
