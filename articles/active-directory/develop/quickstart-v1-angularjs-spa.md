---
title: Создание одностраничного приложения AngularJS для входа и выхода с помощью Azure Active Directory | Документация Майкрософт
description: Узнайте, как создать одностраничное приложение AngularJS, которое выполняет вход через Azure AD и вызывает API-интерфейсы, защищенные в Azure AD, по протоколу OAuth.
services: active-directory
documentationcenter: ''
author: rwike77
manager: CelesteDG
editor: ''
ms.assetid: f2991054-8146-4718-a5f7-59b892230ad7
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: javascript
ms.topic: quickstart
ms.date: 09/24/2018
ms.author: ryanwi
ms.reviewer: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 20c62d379006382d4208e4b111202581bc75454f
ms.sourcegitcommit: 04ec7b5fa7a92a4eb72fca6c6cb617be35d30d0c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/22/2019
ms.locfileid: "68380753"
---
# <a name="quickstart-build-an-angularjs-single-page-app-for-sign-in-and-sign-out-with-azure-active-directory"></a>Краткое руководство. Создание одностраничного приложения AngularJS для входа и выхода с помощью Azure Active Directory

[!INCLUDE [active-directory-develop-applies-v1-adal](../../../includes/active-directory-develop-applies-v1-adal.md)]

Используя Azure Active Directory (Azure AD), разработчики могут не только легко реализовать процедуры входа и выхода пользователей, но и защищать вызовы к API одностраничных приложений по протоколу OAuth. Также разработчик сможет осуществлять аутентификацию пользователей по учетным записям Active Directory Windows Server и использовать любые веб-интерфейсы API, защищенные в Azure AD, включая API Office 365 или API Azure.

Для JavaScript-приложений, выполняющихся в браузере, Azure AD предоставляет библиотеку adal.js, используемую при аутентификации в Active Directory. Adal.js выполняет единственную функцию — упрощает получение маркеров доступа.

В этом кратком руководстве описано, как создать приложение списка задач AngularJS, которое:

* выполняет вход пользователя в приложение, используя Azure AD как поставщика удостоверений;
* Отображает некоторые сведения о пользователе.
* безопасно вызывает реализованный в приложении интерфейс приложения со списком дел, используя токены носителя из Azure AD;
* Обеспечивает функцию выхода пользователя из приложения.

Для создания полного рабочего приложения необходимо сделать следующее.

1. Зарегистрировать приложения в Azure AD.
2. Установить библиотеку ADAL и настроить одностраничное приложение.
3. Применить ADAL для защиты страниц одностраничного приложения.

> [!NOTE]
> Если вы планируете включить вход для личных учетных записей помимо рабочих и учебных учетных записей, можно использовать *[конечную точку платформы удостоверений Майкрософт](azure-ad-endpoint-comparison.md)* . Дополнительные сведения приведены в [этом руководстве по JavaScript SPA](tutorial-v2-javascript-spa.md), а также [в этой статье](active-directory-v2-limitations.md) о *конечной точке платформы удостоверений Майкрософт*. 

## <a name="prerequisites"></a>Предварительные требования

Чтобы начать работу, выполните следующие предварительные требования:

* Скачайте [схему приложения](https://github.com/AzureADQuickStarts/SinglePageApp-AngularJS-DotNet/archive/skeleton.zip) или [готовый пример](https://github.com/AzureADQuickStarts/SinglePageApp-AngularJS-DotNet/archive/complete.zip).
* Убедитесь в наличии клиента Azure AD, в котором можно создать пользователей и зарегистрировать приложение. Если клиента нет, [узнайте, как его получить](quickstart-create-new-tenant.md).

## <a name="step-1-register-the-directorysearcher-application"></a>Шаг 1. Регистрация приложения DirectorySearcher

Чтобы приложение могло осуществлять аутентификацию пользователей и получать маркеры, необходимо зарегистрировать его в клиенте Azure AD.

1. Войдите на [портале Azure](https://portal.azure.com).
1. Если вы вошли в несколько каталогов, убедитесь, что вы просматриваете правильный каталог. Для этого на верхней панели щелкните свою учетную запись. В списке **Каталог** выберите клиент Azure AD для регистрации приложения.
1. В области слева щелкните **Все службы** и выберите **Azure Active Directory**.
1. Щелкните **Регистрация приложений**, а затем выберите **Новая регистрация**.
1. Когда откроется страница **Register an application** (Регистрация приложения), введите имя приложения.
1. В разделе **Поддерживаемые типы учетных записей** выберите **Accounts in any organizational directory and personal Microsoft accounts** (Учетные записи в любом каталоге организации и личные учетные записи Майкрософт).
1. Для платформы выберите тип **Веб** в разделе **URI перенаправления** и установите значение `https://localhost:44326/` (расположение, в которое Azure AD будет возвращать маркеры).
1. По завершении щелкните **Зарегистрировать**. На странице приложения **Обзор** запишите **идентификатор приложения (клиента)** .
1. Библиотека adal.js использует неявный поток OAuth для взаимодействия с Azure AD. Подключите этот неявный поток к своему приложению, выполнив приведенные ниже действия. В левой области навигации зарегистрированного приложения выберите **Проверка подлинности**.
1. В окне **Дополнительные параметры** в разделе **Неявное предоставление** установите флажки **Токен идентификатора** и **Маркеры доступа**. Токены идентификатора и маркеры доступа необходимы, так как это приложение должно выполнять вход пользователей и вызов API.
1. Щелкните **Сохранить**.
1. Предоставьте разрешения для приложения в клиенте. Перейдите на страницу **Разрешения API** и нажмите кнопку **Дать согласие администратора** в разделе **Предоставление согласия**.
1. Выберите **Да** для подтверждения.

## <a name="step-2-install-adal-and-configure-the-single-page-app"></a>Шаг 2. Установка библиотеки ADAL и настройка одностраничного приложения

Теперь, когда приложение зарегистрировано в Azure AD, можно установить библиотеку adal.js и написать код для работы с удостоверением.

### <a name="configure-the-javascript-client"></a>Настройка клиента JavaScript

Добавьте adal.js в проект TodoSPA с помощью консоли диспетчера пакетов.

1. Скачайте файл [adal.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/master/lib/adal.js) и добавьте его в каталог проекта `App/Scripts/`.
2. Скачайте файл [adal-angular.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/master/lib/adal-angular.js) и добавьте его в каталог проекта `App/Scripts/`.
3. До закрывающего тега `</body>` in `index.html`вставьте строки загрузки сценариев:

    ```js
    ...
    <script src="App/Scripts/adal.js"></script>
    <script src="App/Scripts/adal-angular.js"></script>
    ...
    ```

### <a name="configure-the-back-end-server"></a>Настройка внутреннего сервера

Чтобы API серверной части одностраничного интерфейсного приложения со списком дел мог принимать маркеры от браузера, ему требуются сведения о параметрах регистрации приложения. В проекте TodoSPA откройте файл `web.config`. Замените значения элементов в разделе `<appSettings>` на значения, которые вы указали на портале Azure. Код будет использовать эти значения при каждом обращении к библиотеке ADAL.

   * `ida:Tenant` — это имя вашего клиента Azure AD, например contoso.onmicrosoft.com.
   * Для `ida:Audience` укажите скопированный на портале идентификатор клиента приложения.

## <a name="step-3-use-adal-to-help-secure-pages-in-the-single-page-app"></a>Шаг 3. Применение ADAL для защиты страниц одностраничного приложения

Adal.js интегрируется с маршрутом AngularJS и поставщиками HTTP, позволяя вам защищать отдельные представления в одностраничных приложениях.

1. В файл `App/Scripts/app.js` добавьте код для привязки модуля adal.js:

    ```js
    angular.module('todoApp', ['ngRoute','AdalAngular'])
    .config(['$routeProvider','$httpProvider', 'adalAuthenticationServiceProvider',
     function ($routeProvider, $httpProvider, adalProvider) {
    ...
    ```
2. В этом же файле `App/Scripts/app.js` инициализируйте `adalProvider`, используя значения конфигурации для регистрации приложения.

    ```js
    adalProvider.init(
      {
          instance: 'https://login.microsoftonline.com/',
          tenant: 'Enter your tenant name here e.g. contoso.onmicrosoft.com',
          clientId: 'Enter your client ID here e.g. e9a5a8b6-8af7-4719-9821-0deef255f68e',
          extraQueryParameter: 'nux=1',
          //cacheLocation: 'localStorage', // enable this for IE, as sessionStorage does not work for localhost.
      },
      $httpProvider
    );
    ```
3. Защитите представление `TodoList` в приложении, добавив всего одну строку кода: `requireADLogin`.

    ```js
    ...
    }).when("/TodoList", {
            controller: "todoListCtrl",
            templateUrl: "/App/Views/TodoList.html",
            requireADLogin: true,
    ...
    ```

## <a name="summary"></a>Сводка

Теперь у вас есть защищенное одностраничное приложение, которое может обрабатывать вход пользователей и выполнять запросы к серверному API, защищенные с помощью токена носителя. Когда пользователь щелкает ссылку **TodoList**, библиотека adal.js выполняет автоматическое перенаправление к Azure AD для входа, если это необходимо. Кроме того, adal.js автоматически добавляет маркер доступа ко всем запросам AJAX, которые передаются серверной части приложения.

Выше описан минимально возможный процесс создания одностраничного приложения с помощью adal.js. Но для одностраничных приложений есть еще несколько полезных функций.

* Вы можете определить в контроллерах функции, которые будут вызывать библиотеку adal.js для явной обработки запросов на вход и выход. В `App/Scripts/homeCtrl.js` добавьте:

    ```js
    ...
    $scope.login = function () {
        adalService.login();
    };
    $scope.logout = function () {
        adalService.logOut();
    };
    ...
    ```
* В интерфейсе приложения можно предоставить сведения о пользователе. Служба ADAL уже добавлена к контроллеру `userDataCtrl`, поэтому в связанном представлении `App/Views/UserData.html` можно получить доступ к объекту `userInfo`:

    ```js
    <p>{{userInfo.userName}}</p>
    <p>aud:{{userInfo.profile.aud}}</p>
    <p>iss:{{userInfo.profile.iss}}</p>
    ...
    ```

* Также во многих случаях требуется знать, зарегистрирован пользователь или нет. Для сбора этой информации также можно использовать объект `userInfo` . Например, в `index.html` можно отображать кнопку **Вход** или **Выход** в зависимости от текущего состояния аутентификации:

    ```js
    <li><a class="btn btn-link" ng-show="userInfo.isAuthenticated" ng-click="logout()">Logout</a></li>
    <li><a class="btn btn-link" ng-hide=" userInfo.isAuthenticated" ng-click="login()">Login</a></li>
    ```

Интегрированное с Azure AD одностраничное приложение может выполнять аутентификацию пользователей, безопасно обращаться к серверной части по протоколу OAuth 2.0 и получать основные сведения о пользователе. Если же вы этого еще не сделали, пришло время добавить в клиент нескольких пользователей. Запустите одностраничное приложение со списком дел и выполните вход от имени пользователя. Добавьте задачи в список дел этого пользователя, выйдите и войдите снова.

Adal.js позволяет легко добавлять в приложение основные компоненты для работы с идентификаторами. Методы этой библиотеки выполняют всю черновую работу: управляют кэшем, поддерживают протокол OAuth, выводят для пользователя диалоговое окно входа, обновляют маркеры с истекшим сроком действия и многое другое.

Готовый пример (для которого лишь осталось задать конфигурацию) можно найти в [репозитории GitHub](https://github.com/AzureADQuickStarts/SinglePageApp-AngularJS-DotNet/archive/complete.zip).

## <a name="next-steps"></a>Дополнительная информация

Теперь можно приступить к изучению других сценариев.

> [!div class="nextstepaction"]
> [Вызов веб-API CORS из одностраничного приложения](https://github.com/AzureAdSamples/SinglePageApp-WebAPI-AngularJS-DotNet)
