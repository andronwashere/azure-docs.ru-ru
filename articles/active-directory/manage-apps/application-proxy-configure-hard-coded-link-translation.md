---
title: Преобразование ссылок и URL-адресов прокси приложения Azure AD | Документация Майкрософт
description: Основные сведения о соединителях прокси приложения Azure AD.
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/04/2018
ms.author: mimart
ms.reviewer: harshja
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: b0899a127566c4d06de7d42443a956c2660a7a6d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "65956902"
---
# <a name="redirect-hardcoded-links-for-apps-published-with-azure-ad-application-proxy"></a>Перенаправление встроенных ссылок для приложений, опубликованных с помощью прокси приложения Azure AD

С помощью прокси приложения Azure AD ваши локальные приложения доступны удаленным пользователям или пользователям со своими устройствами. Тем не менее, некоторые приложения были разработаны с помощью локальных ссылок, внедренных в HTML-код. Эти ссылки могут работать некорректно при использовании приложения удаленно. Если несколько локальных приложений указывают друг на друга, то пользователям требуется, чтобы ссылки продолжали работать, когда их нет в офисе. 

Чтобы гарантировать работу ссылок как внутри корпоративной сети, так и за ее пределами, лучше всего настроить внешние URL-адреса приложений таким образом, чтобы они совпадали с их внутренними URL-адресами. При настройке внешних URL-адресов используйте [личные домены](application-proxy-configure-custom-domain.md), чтобы вместо домена прокси приложения по умолчанию адреса содержали ваше корпоративное доменное имя.


Если использовать личные домены в клиенте невозможно, следует рассмотреть несколько других вариантов для предоставления этой функциональности. Все они также совместимы с личными доменами и друг с другом, поэтому при необходимости можно настроить личные домены и другие решения. 

**Вариант 1. Использование Managed Browser**. Это решение применимо только в том случае, если вы планируете рекомендовать пользователям или требовать от них, чтобы доступ к приложению осуществлялся через Intune Managed Browser. Он будет обрабатывать все опубликованные URL-адреса. 

**Вариант 2. Использование расширения MyApps**. В этом случае для работы решения пользователи должны установить расширение браузера на стороне клиента. Оно будет обрабатывать все опубликованные URL-адреса и работает с большинством распространенных браузеров. 

**Вариант 3. Использование параметра преобразования ссылок**. Этот параметр задается администратором и недоступен для пользователей. Однако он будет обрабатывать URL-адреса только в HTML и CSS. Жестко заданные внутренние URL-адреса, созданные, к примеру, с помощью JavaScript, работать не будут.  

Благодаря этим особенностям ссылки работают независимо от того, где находятся ваши пользователи. При наличии приложений, которые указывают непосредственно на внутренние конечные точки или порты, эти внутренние URL-адреса можно сопоставить с опубликованными внешними URL-адресами прокси приложения. 

 
> [!NOTE]
> Последний вариант предназначен только для клиентов, которые по какой-либо причине не могут использовать личные домены, имеющие те же внутренние и внешние URL-адреса в своих приложениях. Прежде чем включать эту функцию, проверьте, можете ли вы использовать [личные домены в прокси приложения Azure AD](application-proxy-configure-custom-domain.md). 
> 
> А если преобразование ссылок необходимо настроить в приложении SharePoint, то см. статью [Настройка альтернативных сопоставлений доступа для SharePoint 2013](https://technet.microsoft.com/library/cc263208.aspx), где описан другой подход к сопоставлению ссылок. 

 
### <a name="option-1-intune-managed-browser-integration"></a>Вариант 1. Интеграция с Intune Managed Browser 

Intune Managed Browser можно использовать для обеспечения дополнительной защиты приложения и содержимого. Для работы с таким решением необходимо рекомендовать пользователям или требовать от них, чтобы доступ к приложению осуществлялся через Intune Managed Browser. Все внутренние URL-адреса, опубликованные с помощью Application Proxy, будут распознаваться Managed Browser и перенаправляться на соответствующий внешний URL-адрес. Это гарантирует работоспособность всех жестко заданных внутренних URL-адресов. Кроме того, если пользователь введет внутренний URL-адрес в браузере, этот адрес будет работать всегда, независимо от местонахождения пользователя.  

Дополнительные сведения, включая инструкции по настройке этого варианта, см. в документации по [Managed Browser](https://docs.microsoft.com/intune/app-configuration-managed-browser).  

### <a name="option-2-myapps-browser-extension"></a>Вариант 2. Расширение браузера MyApps 

При использовании расширения браузера MyApps все внутренние URL-адреса, опубликованные с помощью Application Proxy, распознаются и перенаправляются на соответствующий внешний URL-адрес. Это гарантирует работоспособность всех жестко заданных внутренних URL-адресов. Кроме того, если пользователь введет внутренний URL-адрес в браузере, этот адрес будет работать всегда, независимо от местонахождения пользователя.  

Чтобы использовать эту возможность, пользователю необходимо скачать расширение и выполнить вход. Выполнять другие настройки не требуется. 

Дополнительные сведения, включая способы настройки этого параметра см. в разделе [расширение обозревателя MyApps](https://docs.microsoft.com/azure/active-directory/user-help/my-apps-portal-end-user-access#download-and-install-the-my-apps-secure-sign-in-extension) документации.

### <a name="option-3-link-translation-setting"></a>Вариант 3. Параметр преобразования ссылки 

Если включено преобразование ссылок, служба Application Proxy выполняет поиск опубликованных внутренних ссылок в HTML и CSS и преобразует их, чтобы пользователи могли не прерывать работу. При использовании расширения браузера MyApps предпочтительнее Настройка перевода ссылок, так как он предоставляет дополнительные взаимодействие с высокой производительностью для пользователей.

> [!NOTE]
> Если вы используете параметр 2 или 3, только один из них должен быть включен одновременно.

## <a name="how-link-translation-works"></a>Как работает преобразование ссылок

После проверки подлинности, когда прокси-сервер передает данные приложения пользователю, прокси приложения проверяет приложение на наличие встроенных ссылок и заменяет их соответствующими опубликованными внешними URL-адресами.

Прокси приложения предполагает, что приложения имеют кодировку UTF-8. Если это не так, то укажите тип кодировки в HTTP-заголовке ответа, например `Content-Type:text/html;charset=utf-8`.

### <a name="which-links-are-affected"></a>Какие ссылки будут затронуты

Функция преобразования ссылок ищет только ссылки, которые находятся в тегах кода в основной части приложения. Прокси приложения имеет отдельную функцию для преобразования файлов cookie и URL-адресов в заголовках. 

Существует два общих типа внутренних ссылок в локальных приложениях:

- **Относительные внутренние ссылки**, которые указывают на общедоступный ресурс в локальной структуре файлов, такой как `/claims/claims.html`. Эти ссылки автоматически работают в приложениях, которые опубликованы через прокси приложения, и продолжают работать с преобразованием или без него. 
- **Встроенные внутренние ссылки** на другие локальные приложения, такие как `http://expenses`, или опубликованные файлы, например `http://expenses/logo.jpg`. Функция преобразования ссылок работает со встроенными внутренними ссылками и изменяет их для указания на внешние URL-адреса, по которым могут пройти удаленные пользователи.

Полный список тегов кода HTML, что прокси приложения поддерживает преобразование ссылок для метода include:
* a
* audio
* Base
* кнопка
* div
* Внедрить
* Формы
* Кадр
* HEAD
* html
* IFRAME
* Рисунок
* input
* link
* MenuItem
* Meta
* object
* script
* source
* Отслеживание
* video

Кроме того в CSS атрибут URL-адрес также преобразуется.

### <a name="how-do-apps-link-to-each-other"></a>Как приложения ссылаются друг на друга

Преобразование ссылок включено для каждого приложения, чтобы у вас был контроль над работой пользователя на уровне отдельных приложений. Включите преобразование ссылок для приложения, если необходимо, чтобы преобразовывались ссылки *из* этого приложения, а не ссылки *на* него. 

Например, предположим, у вас есть три приложения, опубликованные с помощью прокси приложения. Они содержат ссылки друг на друга: "Преимущества", "Расходы" и "Путешествия". Имеется четвертое приложение, "Отзыв", которое не опубликовано через прокси приложения.

При включении преобразования ссылок для приложения "Преимущества" ссылки на "Расходы" и "Путешествия" перенаправляются на внешние URL-адреса этих приложений, но ссылка на "Отзывы" не перенаправляется, так как у нее нет внешнего URL-адреса. Ссылки из приложений "Расходы" и "Путешествия" обратно в "Преимущества" не работают из-за того, что для этих двух приложений не было включено преобразование.

![Ссылки из приложения "Преимущества" на другие приложения при включенном преобразовании ссылок](./media/application-proxy-configure-hard-coded-link-translation/one_app.png)

### <a name="which-links-arent-translated"></a>Какие ссылки не преобразовываются

Для повышения производительности и безопасности некоторые ссылки не преобразовываются:

- Ссылки вне тегов кода. 
- Ссылки в формате, отличном от HTML или CSS. 
- Ссылки в кодировке URL-адресов.
- Внутренние ссылки, открываемые из других программ. Ссылки, отправленные по электронной почте или через мгновенные сообщения либо включенные в другие документы. Пользователи должны знать, как переходить по внешним URL-адресам.

Если вам необходима поддержка одного из этих двух сценариев, то вместо преобразования ссылок используйте одинаковые внутренние и внешние URL-адреса.  

## <a name="enable-link-translation"></a>Включение преобразования ссылок

Приступить к работе с преобразованием ссылок очень просто:

1. Войдите на [портал Azure](https://portal.azure.com) с использованием учетной записи администратора.
2. Перейдите в **Azure Active Directory** > **Корпоративные приложения** > **Приложения** > приложение, которым необходимо управлять > **Прокси приложения**.
3. Переведите переключатель **Translate URLs in application body** (Преобразовывать URL-адреса в коде приложения) в состояние **Да**.

   ![Выберите "Да" для преобразования URL-адресов в коде приложения](./media/application-proxy-configure-hard-coded-link-translation/select_yes.png)
4. Щелкните **Сохранить**, чтобы применить изменения.

Теперь, когда пользователи получают доступ к этому приложению, прокси будет автоматически проверять внутренние ссылки, которые были опубликованы через прокси приложения на вашем клиенте.

## <a name="send-feedback"></a>Отправка отзывов

Нам нужна ваша помощь, чтобы обеспечить работу функции во всех ваших приложениях. Мы поддерживаем поиск более 30 тегов в HTML и CSS. Если у вас есть пример созданных ссылок, которые не были преобразованы, отправьте фрагмент кода [сюда](mailto:aadapfeedback@microsoft.com). 

## <a name="next-steps"></a>Дальнейшие действия
[Работа с пользовательскими доменами в прокси приложения Azure AD](application-proxy-configure-custom-domain.md)

[Настройка альтернативных сопоставлений доступа для SharePoint 2013](https://technet.microsoft.com/library/cc263208.aspx)
