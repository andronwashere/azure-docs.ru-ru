---
title: Установка расширения браузера для панели доступа к приложениям в Azure | Документация Майкрософт
description: Исправление распространенных ошибок при установке расширения браузера для панели доступа.
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/04/2018
ms.author: mimart
ms.reviewer: japere,asteen
ms.collection: M365-identity-device-management
ms.openlocfilehash: 598a69593046354e83fe35b7b2ecfb0745e8b637
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "66388331"
---
# <a name="install-the-access-panel-browser-extension"></a>Установка расширения браузера для панели доступа

Панель доступа представляет собой веб-портал. Если у вас есть рабочая или учебная учетная запись Azure Active Directory (Azure AD), вы можете использовать панель доступа, чтобы просматривать и запускать облачные приложения, к которым администратор Azure AD предоставил вам доступ. 

Если у вас есть выпуски Azure AD, вы тоже можете использовать возможности самостоятельного управления группами и приложениями с помощью панели доступа. 

Панель доступа можно использовать отдельно от портала Azure. Чтобы ее использовать, вам не нужна подписка Azure.

## <a name="web-browser-requirements"></a>Требования к веб-браузеру

Для работы с панелью доступа требуется браузер с включенной поддержкой JavaScript и CSS. Чтобы пользователь мог входить в приложения через панель доступа с помощью единого входа с паролем, в браузере необходимо установить расширение "Панель доступа". Расширение автоматически загружается при выборе приложения, настроенного на работу с единым входом с помощью пароля.

Для единого входа с паролем можно использовать любой из этих браузеров:

- **Microsoft Edge**: в Windows 10 Anniversary Edition или более поздней версии; 
- **Chrome** — версии для Windows 7 и MacOS X или более поздней версии;
- **Firefox 26.0 или более поздней версии** — в Windows XP с пакетом обновления 2 (SP2) или более поздней версии и в Mac OS X 10.6 или более поздней версии;

## <a name="install-the-access-panel-browser-extension"></a>Установка расширения браузера для панели доступа

Чтобы установить расширение "Панель доступа" для браузера, выполните следующие действия:

1.  Откройте [панель доступа](https://myapps.microsoft.com) в одном из поддерживаемых браузеров и войдите в свою учетную запись пользователя Azure AD.

2.  Выберите приложение единого входа на основе пароля.

3.  Когда появится окно с запросом, выберите **Установить сейчас**.  
    В вашем браузере откроется ссылка для загрузки. 
    
4.  Выберите **Добавить**.

5.  Если появится запрос, **включите** или **разрешите** расширение.

6.  После завершения установки перезапустите браузер.

7.  Войдите в панель доступа и проверьте, можете ли вы запускать приложения с помощью единого входа с паролем.

Вы также можете скачать расширение для Chrome и Microsoft Edge отсюда:

- [Расширение для Chrome](https://chrome.google.com/webstore/detail/access-panel-extension/ggjhpefgjjfobnfoldnjipclpcfbgbhl)
- [Расширение для Microsoft Edge](https://www.microsoft.com/store/apps/9pc9sckkzk84)
- [Расширение Firefox](https://addons.mozilla.org/en-US/firefox/addon/access-panel-extension/)

## <a name="use-the-my-apps-secure-sign-in-extension"></a>Использование расширения защищенного входа на странице "Мои приложения"
* Если для страницы "Мои приложения" вы используете URL-адрес, отличный от `https://myapps.microsoft.com`, настройте URL-адрес по умолчанию следующим образом:
   1. Пока вы *не* вошли в расширение, щелкните правой кнопкой мыши его значок.
   2. Выберите в меню **My Apps URL** (URL-адрес страницы "Мои приложения").
   3. Выберите URL-адрес по умолчанию.
   4. Щелкните значок расширения.
   5. Чтобы войти в расширение, выберите **Sign in to get started** (Войдите, чтобы начать работу).

* Чтобы войти в приложение непосредственно из браузера, сделайте следующее:
   1. Войдите в установленное расширение, выбрав **Sign in to get started** (Войдите, чтобы начать работу).
   2. Войдите в приложение с помощью URL-адреса входа.  
       URL-адрес входа обычно совпадает с URL-адресом приложения, где отображается форма входа.
      Состояние расширения должно измениться, и вы сможете использовать пароль.
   3. Чтобы войти, щелкните значок расширения.

* Чтобы запустить приложение из расширения, сделайте следующее:
   1. Войдите в установленное расширение, выбрав **Sign in to get started** (Войдите, чтобы начать работу).
   2. Щелкните значок расширения, чтобы открыть его меню.
   3. Выполните поиск приложения, которое доступно на портале "Мои приложения".
   4. В списке результатов поиска выберите приложение.  
       В списке **Недавно использованные** отобразятся последние три запущенные приложения.
       
* Чтобы использовать внутренние URL-адреса компании во время удаленного доступа, выполните следующие действия.
    1. [Настройте прокси приложения](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-enable) на вашем клиенте.
    2. [Опубликуйте приложение](https://docs.microsoft.com/azure/active-directory/application-proxy-publish-azure-portal) и URL-адрес с помощью прокси приложения.
    3. Установите расширение и войдите в него, выбрав "Войти", чтобы приступить к работе.
    4. Теперь вы можете просматривать внутренний URL-адрес компании даже при удаленном подключении.

> [!NOTE]
> Описанные выше параметры доступны только в Microsoft Edge, Chrome и Firefox.

## <a name="set-up-a-group-policy-for-internet-explorer"></a>Настройка групповой политики для Internet Explorer

Вы можете настроить групповую политику, которая позволяет удаленно устанавливать расширение "Панель доступа" для Internet Explorer на компьютерах пользователей.

Прежде чем настраивать групповую политику, убедитесь, что вы выполнили следующие требования:

-   Вы уже настроили [доменные службы Active Directory](https://msdn.microsoft.com/library/aa362244%28v=vs.85%29.aspx)и подключили компьютеры пользователей к домену.

-   Чтобы изменять объект групповой политики (GPO), необходимо иметь разрешение *изменять параметры*. По умолчанию это разрешение предоставлено членам следующих групп безопасности: "Администраторы домена", "Администраторы предприятия" и "Владельцы-создатели групповой политики".

Пошаговые инструкции по настройке групповой политики и ее развертыванию для пользователей см. в руководстве по [развертыванию расширения "Панель доступа" для Internet Explorer с помощью групповой политики](deploy-access-panel-browser-extension.md).

## <a name="troubleshoot-the-access-panel-extension-in-internet-explorer"></a>Устранение неполадок расширения "Панель доступа" в Internet Explorer

Сведения о доступе к средству диагностики и инструкции по настройке расширения для Internet Explorer см. в руководстве по [устранению неполадок расширения "Панель доступа" для Internet Explorer](manage-access-panel-browser-extension.md).

> [!NOTE]
> Для браузера Internet Explorer доступна ограниченная поддержка — обновления программного обеспечения больше не доступны. Рекомендуется использовать браузер Microsoft Edge.

## <a name="if-the-preceding-steps-do-not-resolve-the-issue"></a>Если описанные выше шаги не устранят проблему

Создайте запрос в службу поддержки, указав следующие сведения (если они доступны):

-   идентификатор ошибки корреляции;
-   имя участника-пользователя (адрес электронной почты пользователя);
-   идентификатор клиента;
-   тип браузера;
-   часовой пояс и время или интервал времени, когда произошла ошибка;
-   трассировки Fiddler.

## <a name="next-steps"></a>Дальнейшие действия
[Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](what-is-single-sign-on.md)
