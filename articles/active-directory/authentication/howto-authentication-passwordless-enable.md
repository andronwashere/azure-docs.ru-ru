---
title: Настройка Azure Active Directory вход без пароля (Предварительная версия)
description: Включение входа без пароля в Azure AD с помощью ключей безопасности FIDO2 или приложения Microsoft Authenticator (Предварительная версия)
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 07/09/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: librown
ms.collection: M365-identity-device-management
ms.openlocfilehash: ad2f25aac7f74c74eb63fd4666c5184ae751ec1f
ms.sourcegitcommit: a0b37e18b8823025e64427c26fae9fb7a3fe355a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/25/2019
ms.locfileid: "68499933"
---
# <a name="enable-passwordless-sign-in-for-azure-ad-preview"></a>Включение входа без пароля для Azure AD (Предварительная версия)

## <a name="requirements"></a>Требования

* Многофакторная проверка подлинности Azure
* Объединенная Предварительная версия регистрации с включенными пользователями для SSPR
* Для ключа безопасности FIDO2 Предварительная версия требуются совместимые ключи безопасности FIDO2
* Для WebAuthN требуется Microsoft ребро в Windows 10 версии 1809 или более поздней
* Для входа на основе Windows FIDO2 требуется присоединение к Azure AD с Windows 10 версии 1809 или более поздней

## <a name="prepare-devices-for-preview"></a>Подготовка устройств для предварительной версии

Устройства, на которых будет выполняться Пилотная подготовка, должны работать под управлением Windows 10 версии 1809 или более поздней. В Windows 10 версии 1903 или более поздней.

## <a name="enable-security-keys-for-windows-sign-in"></a>Включение ключей безопасности для входа в Windows

Организации могут использовать один или несколько из следующих методов для включения использования ключей безопасности для входа в Windows.

### <a name="enable-credential-provider-via-intune"></a>Включение поставщика учетных данных с помощью Intune

1. Войдите на [портале Azure](https://portal.azure.com).
1. Перейдите к **Microsoft Intune** >  >  > регистрации > устройства регистрация Windows свойства**Windows Hello для бизнеса**.
1. В разделе **Параметры** установите для параметра **Вход использовать ключи безопасности** значение **включено**.

Настройка ключей безопасности для входа не зависит от настройки Windows Hello для бизнеса.

#### <a name="enable-targeted-intune-deployment"></a>Включить целевое развертывание Intune

Чтобы включить поставщик учетных данных для конкретных групп устройств, используйте следующие пользовательские параметры через Intune. 

1. Войдите на [портале Azure](https://portal.azure.com).
1. Перейдите к **Microsoft Intune** >  > **Профили**конфигурации > устройства**создать профиль**.
1. Настройте новый профиль со следующими параметрами.
   1. имя: Ключи безопасности для входа в Windows
   1. Описание: Включает использование ключей безопасности FIDO во время входа Windows
   1. Платформа: Windows 10 и более поздние версии
   1. Тип платформы: Настраиваемый
   1. Настраиваемые параметры OMA-URI:
      1. имя: Включение ключей безопасности FIDO для входа в Windows
      1. OMA-URI:./девице/вендор/мсфт/пасспортфорворк/секуритикэй/усесекуритикэйфорсигнин
      1. Тип данных: Целое число
      1. Значение: 1 
1. Эту политику можно назначить конкретным пользователям, устройствам или группам. Дополнительные сведения можно найти в статье [Назначение профилей пользователей и устройств в Microsoft Intune](https://docs.microsoft.com/intune/device-profile-assign).

![Создание настраиваемой политики конфигурации устройств Intune](./media/howto-authentication-passwordless-enable/intune-custom-profile.png)

### <a name="enable-credential-provider-via-provisioning-package"></a>Включение поставщика учетных данных с помощью пакета подготовки

Для устройств, не управляемых Intune, можно установить пакет подготовки, чтобы включить функциональные возможности. Приложение "конструктор конфигураций Windows" можно установить из [Microsoft Store](https://www.microsoft.com/store/apps/9nblggh4tx22).

1. Запустите конструктор конфигураций Windows.
1. Выберите **файл** > **создать проект**.
1. Присвойте проекту имя и запишите путь, по которому создан проект.
1. Щелкните **Далее**.
1. Оставьте выбранный в качестве **рабочего процесса проекта** **пакет подготовки** и нажмите кнопку **Далее**.
1. Выберите **все выпуски Windows Desktop** в разделе **Выбор параметров для просмотра и настройки** и нажмите кнопку **Далее**.
1. Выберите **Готово**.
1. В созданном проекте перейдите к > параметрам **среды выполнения** **виндовшеллофорбусинесс** > **секуритикэйс** > **усесекуритикэйфорсигнин**.
1. Установите для параметра **усесекуритикэйфорсигнин** значение **Enabled**.
1. Выберите **Экспорт** > **пакета подготовки** .
1. Оставьте значения по умолчанию в окне **Сборка** в разделе **Описание пакета подготовки** и нажмите кнопку **Далее**.
1. Оставьте значения по умолчанию в окне **Сборка** в разделе **выберите сведения о безопасности для пакета подготовки** и нажмите кнопку **Далее**.
1. Запишите или измените путь в окнах **Сборка** в разделе **Выбор места сохранения пакета подготовки** и нажмите кнопку **Далее**.
1. Выберите **Сборка** на странице **Построение пакета подготовки** .
1. Сохраните два созданных файла (ppkg и CAT) в расположении, где их можно будет применить к компьютерам позже.
1. Следуйте указаниям в статье [Применение пакета подготовки](https://docs.microsoft.com/windows/configuration/provisioning-packages/provisioning-apply-package)для применения созданного пакета подготовки.

## <a name="obtain-fido2-security-keys"></a>Получение ключей безопасности FIDO2

Дополнительные сведения о поддерживаемых ключах и производителях см. в разделе FIDO2 Security Keys в статье [что такое пароль?](concept-authentication-passwordless.md) .

> [!NOTE]
> При покупке и планировании использования ключей безопасности на основе NFC вам потребуется поддерживаемый модуль чтения NFC.

## <a name="enable-passwordless-authentication-methods"></a>Включить методы проверки подлинности, не имеющие пароля

### <a name="enable-the-combined-registration-experience"></a>Включение объединенного процесса регистрации

Функции регистрации для ключей безопасности FIDO2 полагаются на Объединенную предварительную версию регистрации. Выполните следующие действия, чтобы включить предварительную версию общей регистрации.

1. Войдите на [портал Azure](https://portal.azure.com)
1. Выберите **Azure Active Directory** > **Параметры пользователя**
   1. Щелкните " **Управление параметрами предварительной версии функции пользователя** "
   1. В разделе **пользователи могут использовать функции предварительной версии для регистрации и управления сведениями о безопасности**.
      1. Выберите **выбранное** и выберите группу пользователей, которые будут участвовать в предварительной версии.
      1. Или выберите **все** , чтобы включить для всех пользователей в каталоге.
1. Нажмите кнопку **Сохранить**

### <a name="enable-new-passwordless-authentication-methods"></a>Включить новые методы проверки подлинности, не имеющие пароля

1. Войдите на [портал Azure](https://portal.azure.com)
1. Перейти к **Azure Active Directory** > **методы** > проверки подлинности проверка**политики метода аутентификации (Предварительная версия)**
1. В каждом **методе**выберите следующие параметры.
   1. **Включить** -да или нет
   1. **Target** — все пользователи или выбранные пользователи
1. **Сохранение** каждого метода

> [!WARNING]
> FIDO2 "политики ограничения ключей" еще не работают. Эта функция будет доступна до выхода общедоступной системы. не изменяйте эти политики по умолчанию.

> [!NOTE]
> Не нужно использовать оба метода без пароля (если требуется предварительный просмотр только одного метода без пароля), можно включить только этот метод. Мы рекомендуем испытать оба метода, так как они имеют свои преимущества.

## <a name="user-registration-and-management-of-fido2-security-keys"></a>Регистрация пользователей и управление ключами безопасности FIDO2

1. Перейдите на сайт [https://myprofile.microsoft.com](https://myprofile.microsoft.com).
1. Войти, если он еще не создан
1. Щелкните **сведения о безопасности**
   1. Если у пользователя уже есть по крайней мере один зарегистрированный метод многофакторной идентификации Azure, он может немедленно зарегистрировать ключ безопасности FIDO2.
   1. Если у них нет хотя бы одного метода многофакторной идентификации Azure, он должен добавить один из них.
1. Добавьте ключ безопасности FIDO2, нажав кнопку **Добавить метод** и выбрав **ключ безопасности** .
1. Выберите **USB-устройство** или **устройство NFC**
1. Приготовьте ключ и нажмите кнопку " **Далее** "
1. Появится поле с просьбой создать или ввести ПИН-код для ключа безопасности, а затем выполнить требуемый жест для ключа как биометрической или Touch.
1. Вы вернетесь к объединенному интерфейсу регистрации и запросили понятное имя для маркера, чтобы можно было указать, какой из них имеет несколько. Нажмите кнопку **Далее**.
1. Нажмите кнопку **Готово** , чтобы завершить процесс.

### <a name="manage-security-key-biometric-pin-or-reset-security-key"></a>Управление биометрической метрикой, ПИН-кодом или сбросом ключа безопасности

* Windows 10, версия 1809
   * Требуется сопутствующее программное обеспечение от поставщика ключа безопасности
* Windows 10 версии 1903 или более поздней
   * Пользователи могут открывать **Параметры Windows** на **своем >ном** > **ключе безопасности** устройства.
   * Пользователи могут изменить свой ПИН-код, обновить биометрию или сбросить ключ безопасности

## <a name="user-registration-and-management-of-microsoft-authenticator-app"></a>Регистрация пользователей и управление Microsoft Authenticator приложением

Чтобы настроить Microsoft Authenticator приложение для входа на телефоне, следуйте указаниям в статье вход в [учетные записи с помощью приложения Microsoft Authenticator](../user-help/user-help-auth-app-sign-in.md).

## <a name="sign-in-with-passwordless-credentials"></a>Вход с использованием учетных данных без пароля

### <a name="sign-in-at-the-lock-screen"></a>Вход на экран блокировки

В примере ниже пользователь Bala Сандху уже предоставил свой ключ безопасности FIDO2. Bala может выбрать поставщик учетных данных ключа безопасности на экране блокировки Windows 10 и вставить ключ безопасности для входа в Windows.

![Вход с помощью ключа безопасности на экране блокировки Windows 10](./media/howto-authentication-passwordless-enable/fido2-windows-10-1903-sign-in-lock-screen.png)

### <a name="sign-in-on-the-web"></a>Вход в Интернет

В примере ниже пользователь уже предоставил свой ключ безопасности FIDO2. Пользователь может выбрать вход в Интернет с помощью ключа безопасности FIDO2 в браузере Microsoft ребра в Windows 10 версии 1809 или более поздней.

![Вход с ключом безопасности в Microsoft ребро](./media/howto-authentication-passwordless-enable/fido2-windows-10-1903-edge-sign-in.png)

Сведения о входе в систему с помощью Microsoft Authenticatorного приложения см. в статье [Вход в учетные записи с помощью приложения Microsoft Authenticator](../user-help/user-help-auth-app-sign-in.md).

## <a name="known-issues"></a>Известные проблемы

### <a name="fido2-security-keys"></a>Ключи безопасности FIDO2

#### <a name="security-key-provisioning"></a>Подготовка ключа безопасности

В общедоступной предварительной версии недоступна подготовка администратора и Отмена подготовки ключей безопасности.

#### <a name="hybrid-azure-ad-join"></a>Гибридное присоединение устройств к Azure AD

Пользователи полагается на единый вход WIA, использующий управляемые учетные данные, такие как ключи безопасности FIDO2 или вход без пароля, с Microsoft Authenticator приложению требуется гибридное соединение в Windows 10, чтобы получить преимущества единого входа. Однако ключи безопасности работают только для Azure Active Directory присоединенных компьютеров. Мы рекомендуем испытать ключи безопасности FIDO2 только для экрана блокировки Windows на чисто Azure Active Directory Соединенных компьютерах. Это ограничение не распространяется на веб-сайт.

#### <a name="upn-changes"></a>Изменения имени участника-пользователя

Мы работаем над поддержкой функции, которая разрешает изменение имени участника-пользователя на гибридных устройствах AADJ и AADJ. В случае изменения имени участника-пользователя вы больше не сможете изменить ключи безопасности FIDO2, чтобы учитывать их. Поэтому единственным подходом является сброс устройства и повторная регистрация пользователя.

### <a name="authenticator-app"></a>Приложение Authenticator

#### <a name="ad-fs-integration"></a>Интеграция AD FS

Когда пользователь включит учетные данные Microsoft Authenticator без пароля, аутентификация этого пользователя по умолчанию всегда будет выполняться через уведомление для подтверждения. Эта логика запрещает пользователям в гибридном клиенте направляться в ADFS для проверки входа, не выполняя дополнительные действия по нажатию кнопки "использовать пароль". Этот процесс также будут обходить любые локальные политики условного доступа и потоки сквозной проверки подлинности. Исключением из этого процесса является то, что указан login_hint, пользователь будет перенаправлен на AD FS и обойти параметр, чтобы использовать учетные данные без пароля.

#### <a name="azure-mfa-server"></a>Сервер Azure MFA

Конечные пользователи, которые поддерживают MFA через локальный сервер Azure MFA Организации, могут по-прежнему создавать и использовать один и тот же учетные данные для входа в систему без пароля. Если пользователь введет одну учетную запись в несколько (5 и более) установок Microsoft Authenticator, это может привести к ошибке.  

#### <a name="device-registration"></a>Регистрация устройства

Одним из обязательных условий для создания защищенных учетных данных нового типа является регистрация устройства, на котором они размещаются, в клиенте Azure AD для конкретного пользователя. Ограничения по регистрации устройств позволяют зарегистрировать устройство только в одном клиенте. Это ограничение означает, что для входа с помощью телефона можно включить только одну рабочую или учебную учетную запись в Microsoft Authenticator приложении.

## <a name="next-steps"></a>Следующие шаги

[Сведения о регистрации устройств](../devices/overview.md)

[Сведения о многофакторной идентификации Azure](../authentication/howto-mfa-getstarted.md)
