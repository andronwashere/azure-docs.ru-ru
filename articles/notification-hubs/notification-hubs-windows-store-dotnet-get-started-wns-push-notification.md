---
title: Отправка уведомлений в приложения универсальной платформы Windows с использованием Центров уведомлений Azure | Документация Майкрософт
description: Узнайте, как отправлять push-уведомления в приложение для универсальной платформы Windows с помощью Центров уведомлений Azure.
services: notification-hubs
documentationcenter: windows
author: jwargo
manager: patniko
editor: spelluru
ms.assetid: cf307cf3-8c58-4628-9c63-8751e6a0ef43
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows
ms.devlang: dotnet
ms.topic: tutorial
ms.custom: mvc
ms.date: 03/22/2019
ms.author: jowargo
ms.openlocfilehash: 714c2be7265a41a7791ce39bfaa1bdc12fa58cac
ms.sourcegitcommit: a12b2c2599134e32a910921861d4805e21320159
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/24/2019
ms.locfileid: "67340749"
---
# <a name="tutorial-send-notifications-to-universal-windows-platform-apps-by-using-azure-notification-hubs"></a>Руководство по Отправка уведомлений в приложения универсальной платформы Windows с использованием Центров уведомлений Azure

[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

В этом руководстве создается Центр уведомлений Azure для отправки push-уведомлений в приложение универсальной платформы Windows (UWP). Вы создадите пустое приложение Магазина Windows, получающее push-уведомления с помощью службы push-уведомлений Windows (WNS). Затем вы сможете рассылать push-уведомления на все устройства, на которых запущено ваше приложение, с помощью центра уведомлений.

> [!NOTE]
> Полный код для работы с этим руководством можно найти на портале [GitHub](https://github.com/Azure/azure-notificationhubs-dotnet/tree/master/Samples/UwpSample).

Вам необходимо сделать следующее:

> [!div class="checklist"]
> * Создание приложения в Магазине Windows
> * Создание центра уведомлений
> * Создание примера приложения Windows
> * Отправка проверочных уведомлений

## <a name="prerequisites"></a>Предварительные требования

- **Подписка Azure**. Если у вас еще нет подписки Azure, создайте [бесплатную учетную запись Azure](https://azure.microsoft.com/free/?WT.mc_id=A261C142F), прежде чем начинать работу.
- [Microsoft Visual Studio Community 2015](https://www.visualstudio.com/products/visual-studio-community-vs) или более поздняя версия.
- [Установленные средства разработки приложений универсальной платформы Windows](https://msdn.microsoft.com/windows/uwp/get-started/get-set-up)
- Активная учетная запись Магазина Windows.
- Убедитесь, что параметр **Получать уведомления от приложений и других отправителей** включен. 
    - Откройте окно **Параметры** на компьютере.
    - Выберите элемент **Система**.
    - Выберите **Notifications & actions** (Уведомления и действия) в меню слева. 
    - Убедитесь, что параметр **Получать уведомления от приложений и других отправителей** включен. Если он не включен, включите его. 

Изучение этого руководства является необходимым условием для работы со всеми остальными руководствами, посвященными Центрам уведомлений для приложений универсальной платформы Windows.

## <a name="create-an-app-in-windows-store"></a>Создание приложения в Магазине Windows

Чтобы отправлять push-уведомления в приложения UWP, необходимо связать приложение с Магазином Windows. Затем необходимо настроить интеграцию концентратора уведомлений с WNS.

1. Перейдите в [Центр разработки для Windows](https://partner.microsoft.com/en-us/dashboard/windows/first-run-experience), войдите с помощью учетной записи Майкрософт, а затем выберите **Создать приложение**.

    ![Кнопка "Создать приложение"](./media/notification-hubs-windows-store-dotnet-get-started/windows-store-new-app-button.png)
2. Введите имя приложения и выберите **Зарезервировать имя продукта**. Будет создана регистрация в Магазине Windows для вашего приложения.

    ![Имя приложения в магазине](./media/notification-hubs-windows-store-dotnet-get-started/store-app-name.png)
3. Разверните **Управление приложениями**, выберите **WNS/MPNS**, а затем — **сайт служб Live**. Войдите в систему с помощью учетной записи Майкрософт. **Портал регистрации приложений** откроется в новой вкладке. Вы также можете напрямую перейти к [порталу регистрации приложений](https://apps.dev.microsoft.com) и выбрать имя приложения, чтобы открылась эта страница.

    ![Страница WNS MPNS](./media/notification-hubs-windows-store-dotnet-get-started/wns-mpns-page.png)
4. Запишите значение пароля в разделе **секрета приложения** и значение **идентификатора безопасности пакета (SID)** .

    >[!WARNING]
    >Секрет приложения и ИД безопасности пакета — это важные учетные данные для безопасного доступа. Не сообщайте никому эти значения и не распространяйте их вместе со своим приложением.

## <a name="create-a-notification-hub"></a>Создание центра уведомлений

[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

### <a name="configure-wns-settings-for-the-hub"></a>Настройка параметров WNS для центра

1. Выберите **Windows (WNS)** в категории **Параметры уведомлений**.
2. Введите значения для **ИД безопасности пакета** и **ключа безопасности**, которые были записаны в предыдущем разделе.
3. Нажмите кнопку **Сохранить** на панели инструментов.

    ![Поля "Идентификатор безопасности пакета" и "Ключ безопасности"](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-configure-wns.png)

Теперь концентратор уведомлений настроен для работы с WNS. А у вас есть строки подключения для регистрации приложения и отправки уведомлений.

## <a name="create-a-sample-windows-app"></a>Создание примера приложения Windows

1. В Visual Studio откройте меню **Файл** и выберите **Создать**, а затем — **Проект**.
2. В диалоговом окне **Создать проект** сделайте следующее:

    1. Разверните **Visual C#** .
    2. Выберите **Универсальные приложения Windows**.
    3. Выберите **Пустое приложение (универсальное приложение Windows)** .
    4. Введите **имя** проекта.
    5. Нажмите кнопку **ОК**.

        ![Диалоговое окно "Новый проект"](./media/notification-hubs-windows-store-dotnet-get-started/new-project-dialog.png)
3. Примите значения по умолчанию для **целевой** и **минимальной** версий платформы и нажмите кнопку **ОК**.
4. В обозревателе решений щелкните правой кнопкой мыши проект приложения для Магазина Windows, выберите **Магазин**, а затем — **Связать приложение с Магазином**. Откроется мастер **Свяжите свое приложение с Магазином Windows** .
5. В мастере войдите с помощью учетной записи Майкрософт.
6. Выберите приложение, зарегистрированное на шаге 2, нажмите кнопку **Далее**, а затем выберите **Связать**. После этого необходимые регистрационные данные Магазина Windows будут добавлены в манифест приложения.
7. В Visual Studio щелкните решение правой кнопкой мыши, а затем выберите пункт **Управление пакетами NuGet**. Откроется окно **Управление пакетами NuGet**.
8. В поле поиска введите **WindowsAzure.Messaging.Managed**, затем выберите **установить** и примите условия использования.

    ![Окно "Управление пакетами NuGet"][20]

    Это действие предназначено для скачивания и установки библиотеки Центров уведомлений Azure для Windows, а также добавления ссылки на нее с помощью [пакета NuGet Microsoft.Azure.Notification Hubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs).
9. Откройте файл проекта `App.xaml.cs` и добавьте следующие операторы:

    ```csharp
    using Windows.Networking.PushNotifications;
    using Microsoft.WindowsAzure.Messaging;
    using Windows.UI.Popups;
    ```

10. В файле проекта `App.xaml.cs` найдите класс `App` и добавьте следующее определение метода `InitNotificationsAsync`:

    ```csharp
    private async void InitNotificationsAsync()
    {
        var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

        var hub = new NotificationHub("<your hub name>", "<Your DefaultListenSharedAccessSignature connection string>");
        var result = await hub.RegisterNativeAsync(channel.Uri);

        // Displays the registration ID so you know it was successful
        if (result.RegistrationId != null)
        {
            var dialog = new MessageDialog("Registration successful: " + result.RegistrationId);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();
        }
    }
    ```

    Этот код получает универсальный идентификатор ресурса (URI) канала для приложения из WNS, а затем регистрирует этот идентификатор в вашем центре уведомлений.

    >[!NOTE]
    > Замените заполнитель `hub name` именем концентратора уведомлений, которое отображается на портале Azure. Кроме того, замените заполнитель строки подключения строкой подключения `DefaultListenSharedAccessSignature`, полученной на странице **Политики доступа** концентратора уведомлений при работе с предыдущим разделом.

11. В верхней части обработчика событий `OnLaunched` в файле `App.xaml.cs` добавьте следующий вызов к новому методу `InitNotificationsAsync`:

    ```csharp
    InitNotificationsAsync();
    ```

    Это обеспечит регистрацию URI канала в центре уведомлений при каждом запуске приложения.

12. Чтобы запустить приложение, нажмите на клавиатуре клавишу **F5**. Отобразится диалоговое окно, содержащее ключ регистрации. Чтобы закрыть диалоговое окно, нажмите кнопку**ОК**.

    ![Регистрация выполнена успешно](./media/notification-hubs-windows-store-dotnet-get-started/registration-successful.png)

Теперь приложение готово к получению всплывающих уведомлений.

## <a name="send-test-notifications"></a>Отправка проверочных уведомлений

Чтобы проверить получение уведомлений в приложении, отправьте уведомления на [портале Azure](https://portal.azure.com/).

1. На портале Azure перейдите на вкладку "Обзор" и выберите **Тестовая отправка** на панели инструментов.

    ![Кнопка "Тестовая отправка"](./media/notification-hubs-windows-store-dotnet-get-started/test-send-button.png)
2. В окне **Тестовая отправка** сделайте следующее:
    1. В поле **Платформы** выберите значение **Windows**.
    2. В поле **Тип уведомления** выберите значение **Всплывающее уведомление**.
    3. Нажмите кнопку **Отправить**.

        ![Область тестовой отправки](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-test-send-wns.png)
3. Просмотреть результат операции отправки можно в списке **Результат** в нижней части окна. Также появится оповещение.

    ![Результат операции отправки](./media/notification-hubs-windows-store-dotnet-get-started/result-of-send.png)
4. Вы увидите оповещение **Тестовое сообщение** на своем рабочем столе.

    ![Сообщение с уведомлением](./media/notification-hubs-windows-store-dotnet-get-started/test-notification-message.png)

## <a name="next-steps"></a>Дополнительная информация
Вы настроили рассылку уведомлений на все устройства Windows через портал или консольное приложение. Чтобы узнать, как отправлять push-уведомления на конкретные устройства, перейдите к следующему руководству:

> [!div class="nextstepaction"]
>[Руководство по отправке push-уведомлений на конкретные устройства Android с помощью Центров уведомлений Azure и Google Cloud Messaging](
notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md)

<!-- Images. -->
[13]: ./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-create-console-app.png
[14]: ./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-windows-toast.png
[19]: ./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-windows-reg.png
[20]: ./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-windows-universal-app-install-package.png

<!-- URLs. -->
[Use Notification Hubs to push notifications to users]: notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md
[Use Notification Hubs to send breaking news]: notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md
[toast catalog]: https://msdn.microsoft.com/library/windows/apps/hh761494.aspx
[tile catalog]: https://msdn.microsoft.com/library/windows/apps/hh761491.aspx
[badge overview]: https://msdn.microsoft.com/library/windows/apps/hh779719.aspx
