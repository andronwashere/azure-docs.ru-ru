---
title: Начало работы с проверкой подлинности для мобильных приложений Xamarin для Android
description: Узнайте, как использовать мобильные приложения для проверки подлинности пользователей приложения Xamarin для Android с помощью разных поставщиков удостоверений, включая AAD, Google, Facebook, Twitter и Майкрософт.
services: app-service\mobile
documentationcenter: xamarin
author: elamalani
manager: panarasi
editor: ''
ms.assetid: 570fc12b-46a9-4722-b2e0-0d1c45fb2152
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin-android
ms.devlang: dotnet
ms.topic: article
ms.date: 06/25/2019
ms.author: emalani
ms.openlocfilehash: 7a0b2c54c2d2a9daba56ea1d05c18e72a2d7a7a0
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/28/2019
ms.locfileid: "67447050"
---
# <a name="add-authentication-to-your-xamarinandroid-app"></a>Добавление проверки подлинности в приложение Xamarin.Android
[!INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

> [!NOTE]
> Центр приложений Visual Studio вкладывает средства в новые и интегрированной службы, необходимые для разработки мобильных приложений. Разработчики могут использовать **построения**, **теста** и **распределить** служб для настройки конвейера непрерывной интеграции и доставки. После развертывания приложения, разработчики могут отслеживать состояние и использования их приложений с помощью **Analytics** и **диагностики** служб и общайтесь с пользователями, с помощью **Push** Служба. Разработчики также могут использовать **Auth** подлинность пользователей и **данных** службы для сохранения и синхронизировать данные приложения в облаке. Ознакомьтесь с [App Center](https://appcenter.ms/?utm_source=zumo&utm_campaign=app-service-mobile-xamarin-android-get-started-users) уже сегодня.
>

## <a name="overview"></a>Обзор
В этом разделе показано, как аутентифицировать пользователей мобильного приложения из клиентского приложения. В этом учебнике вы добавите аутентификацию в проект быстрого запуска, используя поставщик удостоверений, поддерживаемый мобильными приложениями Azure. После успешной аутентификации и авторизации в мобильном приложении отображается значение идентификатора пользователя.

Этот учебник создан на основе краткого руководства по мобильным приложениям. Необходимо также сначала пройти руководство [Создание приложения Xamarin.Android]. Если вы не используете скачанный проект быстрого запуска сервера, в проект необходимо добавить пакет расширений для аутентификации. в статье [Работа с пакетом SDK для внутреннего сервера .NET для мобильных приложений Azure](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

## <a name="register"></a>Регистрация приложения для проверки подлинности и настройка служб приложений
[!INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

## <a name="redirecturl"></a>Добавление приложения в список разрешенных URL-адресов внешнего перенаправления

Для безопасной аутентификации требуется определить новую схему URL-адресов для своего приложения. Это позволяет системе аутентификации выполнять перенаправление обратно в приложение после завершения процесса аутентификации. В этом руководстве мы повсеместно используем схему URL-адресов _appname_. Тем не менее можно использовать любую схему URL-адресов на свой выбор. Она должна быть уникальной для мобильного приложения. Вот как можно включить перенаправление на стороне сервера.

1. На портале Azure выберите свою службу приложений.

2. Выберите пункт меню **Аутентификация или авторизация**.

3. В поле **Разрешенные URL-адреса внешнего перенаправления** введите `url_scheme_of_your_app://easyauth.callback`.  **url_scheme_of_your_app** в этой строке — это схема URL-адресов для вашего мобильного приложения.  Она должна соответствовать обычной спецификации URL-адресов для протокола (можно использовать буквы и цифры, и адрес должен начинаться с буквы).  Необходимо записать выбранную строку, так как потребуется в нескольких местах настроить код мобильного приложения с использованием схемы URL-адресов.

4. Последовательно выберите **ОК**.

5. Выберите команду **Сохранить**.

## <a name="permissions"></a>Предоставление разрешений только пользователям, прошедшим проверку подлинности
[!INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

В Visual Studio или Xamarin Studio запустите клиентский проект на устройстве или в эмуляторе. Убедитесь, что после запуска приложения возникает необработанное исключение с кодом состояния 401 (неавторизованный). Это вызвано тем, что приложение пытается получить доступ к серверной части мобильного приложения от имени неаутентифицированного пользователя. Теперь для таблицы *TodoItem* требуется аутентификация.

Далее вы обновите клиентское приложение для запроса ресурсов из серверной части мобильного приложения прошедшим аутентификацию пользователем.

## <a name="add-authentication"></a>Добавление проверки подлинности в приложение
Для отображения данных в обновленном приложении пользователи должны будут коснуться кнопки **Вход** и пройти проверку подлинности.

1. Добавьте в класс **TodoActivity** следующий код:
   
        // Define an authenticated user.
        private MobileServiceUser user;
        private async Task<bool> Authenticate()
        {
                var success = false;
                try
                {
                    // Sign in with Facebook login using a server-managed flow.
                    user = await client.LoginAsync(this,
                        MobileServiceAuthenticationProvider.Facebook, "{url_scheme_of_your_app}");
                    CreateAndShowDialog(string.Format("you are now logged in - {0}",
                        user.UserId), "Logged in!");
   
                    success = true;
                }
                catch (Exception ex)
                {
                    CreateAndShowDialog(ex, "Authentication failed");
                }
                return success;
        }
   
        [Java.Interop.Export()]
        public async void LoginUser(View view)
        {
            // Load data only after authentication succeeds.
            if (await Authenticate())
            {
                //Hide the button after authentication succeeds.
                FindViewById<Button>(Resource.Id.buttonLoginUser).Visibility = ViewStates.Gone;
   
                // Load the data.
                OnRefreshItemsSelected();
            }
        }
   
    При этом создается новый метод для проверки подлинности пользователя и метод обработчика для новой кнопки **Вход** . Пользователь в примере кода выше аутентифицируется с помощью имени для входа Facebook. Диалоговое окно используется для отображения идентификатора пользователя после аутентификации.
   
   > [!NOTE]
   > Если вы используете поставщик удостоверений, отличный от Facebook, измените значение, передаваемое **LoginAsync** выше на одно из следующих: *MicrosoftAccount*, *Twitter*, *Google* или *WindowsAzureActiveDirectory*.
   > 
   > 
2. В методе **OnCreate** удалите или закомментируйте следующую строку кода:
   
        OnRefreshItemsSelected ();
3. В файле Activity_To_Do.axml вставьте определение кнопки *LoginUser* перед уже имеющейся кнопкой *AddItem*:
   
          <Button
            android:id="@+id/buttonLoginUser"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:onClick="LoginUser"
            android:text="@string/login_button_text" />
4. Добавьте следующий элемент в файл ресурсов Strings.xml:
   
        <string name="login_button_text">Sign in</string>
5. Откройте файл AndroidManifest.xml и добавьте следующий код в XML-элемент `<application>`:

        <activity android:name="com.microsoft.windowsazure.mobileservices.authentication.RedirectUrlActivity" android:launchMode="singleTop" android:noHistory="true">
          <intent-filter>
            <action android:name="android.intent.action.VIEW" />
            <category android:name="android.intent.category.DEFAULT" />
            <category android:name="android.intent.category.BROWSABLE" />
            <data android:scheme="{url_scheme_of_your_app}" android:host="easyauth.callback" />
          </intent-filter>
        </activity>

6. В Visual Studio или Xamarin Studio запустите клиентский проект на устройстве или в эмуляторе либо выполните вход с помощью выбранного поставщика удостоверений. После успешного выполнения входа в приложении отобразится ваш идентификатор для входа и список элементов задач, и вы сможете внести изменения в данные.

## <a name="troubleshooting"></a>Устранение неполадок

**Сбой приложения с ошибкой `Java.Lang.NoSuchMethodError: No static method startActivity`**

В некоторых случаях конфликты в пакетах поддержки отображаются в виде обычного предупреждения в Visual Studio, но во время выполнения происходит сбой приложения с этим исключением. В этом случае необходимо убедиться, что у всех пакетов поддержки, на которые содержатся ссылки в проекте, одинаковая версия. [Пакет NuGet для мобильных приложений Azure](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/) имеет зависимость `Xamarin.Android.Support.CustomTabs` для платформы Android, поэтому если в проекте используются пакеты поддержки более новой версии, во избежание конфликтов необходимо установить требуемую версию пакета.

<!-- URLs. -->
[Создание приложения Xamarin.Android]: app-service-mobile-xamarin-android-get-started.md
