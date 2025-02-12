---
title: Настройка интерфейса пути взаимодействия пользователя с помощью настраиваемых политик | Документация Майкрософт
description: Сведения о пользовательских политиках Azure Active Directory B2C.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 04/25/2017
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 7053f5b0211878d2f0b9d810fc3f4c0b9361e6f7
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "66509603"
---
# <a name="customize-the-ui-of-a-user-journey-with-custom-policies"></a>Настройка пользовательского интерфейса с помощью настраиваемых политик

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

> [!NOTE]
> В этой статье представлено расширенное описание принципов настройки пользовательского интерфейса и включения пользовательских политик Azure AD B2C с помощью инфраструктуры процедур идентификации.


Удобная работа пользователя является ключевым фактором для любого корпоративного решения, предоставляемого потребителям. Под удобной работой пользователя подразумевается работа на устройстве или в браузере, при которой взаимодействие пользователя с предоставляемой службой невозможно отличить от взаимодействия со службой, которую он использует.

## <a name="understand-the-cors-way-for-ui-customization"></a>Общие сведения о механизме CORS для настройки пользовательского интерфейса

Azure AD B2C позволяет настраивать внешний вид и поведение пользовательского интерфейса на разных страницах, которые обслуживаются и отображаются в Azure AD B2C с помощью пользовательских политик.

Для этого Azure AD B2C выполняет код в браузере пользователя и использует современный стандартный подход [общего доступа к ресурсам независимо от источника (CORS)](https://www.w3.org/TR/cors/) для загрузки пользовательского содержимого с определенного URL-адреса, указанного в пользовательской политике, чтобы она указывала на шаблоны HTML5/CSS. Механизм CORS позволяет запрашивать ограниченные ресурсы на веб-странице (например, шрифты) из домена, отличного от домена исходного размещения ресурсов.

По сравнению с традиционным механизмом, когда страницы шаблонов принадлежат решению с небольшим количеством текста и изображений и ограниченной настройкой макета и внешнего вида, что усложняло работу, способ CORS поддерживает HTML5 и CSS и предоставляет следующие возможности:

- Размещение содержимого и внедрение элементов управления решения с помощью клиентского скрипта.
- Полный контроль над каждым пикселем и внешним видом макета.

Вы можете предоставить любое количество страниц содержимого, составляя файлы HTML5 и CSS соответствующим образом.

> [!NOTE]
> По соображениям безопасности использование JavaScript для настройки сейчас заблокировано. 

В каждом шаблоне HTML5 и CSS предоставляется элемент *привязки*, который соответствует необходимому элементу `<div id=”api”>` в HTML или на странице содержимого, как показано далее. Для Azure AD B2C необходимо, чтобы все страницы содержимого имели этот div.

```
<!DOCTYPE html>
<html>
  <head>
    <title>Your page content’s tile!</title>
  </head>
  <body>
    <div id="api"></div>
  </body>
</html>
```

Содержимое страницы, связанное с Azure AD B2C, внедряется в этот div, а остальной частью страницы вы сможете управлять сами. Код JavaScript Azure AD B2C получает содержимое и внедряет HTML в этот элемент div. Azure AD B2C внедряет следующие элементы управления соответствующим образом: элемент управления средства выбора учетной записи, элементы управления входа, элементы управления многофакторной проверки подлинности (сейчас для телефона) и элементы управления коллекции атрибутов. Azure AD B2C гарантирует, что все элементы управления совместимы с HTML5, доступны и к ним можно применить стили, а также гарантирует, что версии элементов управления не устареют.

В конечном счете для пользователя объединенное содержимое отображается как динамический документ.

Чтобы удостовериться, что все работает ожидаемым образом, необходимо сделать следующее:

- Убедитесь, что содержимое HTML5 соответствует и доступно.
- Убедитесь, что на сервере содержимого включен механизм CORS.
- Передавайте содержимое по протоколу HTTPS.
- Используйте абсолютные URL-адреса для всех ссылок и содержимого CSS, например `https://yourdomain/content`.

> [!TIP]
> Чтобы проверить, включен ли CORS на сайте, на котором размещается содержимое, и протестировать запросы CORS, можно использовать сайт https://test-cors.org/. Благодаря данному веб-сайту можно отправить запрос CORS к удаленному серверу (для проверки поддержки CORS) или к тестовому серверу (для просмотра определенных функций CORS).

> [!TIP]
> На сайте https://enable-cors.org/ также представлены полезные ресурсы со сведениями о CORS.

Благодаря подходу на основе CORS конечные пользователи получают постоянный опыт, работая и в приложении, и на страницах, обслуживаемых Azure AD B2C.

## <a name="create-a-storage-account"></a>Создание учетной записи хранения

Перед началом необходимо создать учетную запись хранения. Для создания учетной записи хранения больших двоичных объектов Azure требуется подписка Azure. Вы можете оформить подписку на бесплатную пробную версию на [веб-сайте Azure](https://azure.microsoft.com/pricing/free-trial/).

1. Откройте браузер и перейдите на [портал Azure](https://portal.azure.com).
2. Выполните вход с использованием учетных данных администратора.
3. Щелкните **Создать ресурс** > **Хранилище** > **Учетная запись хранения**.  Откроется панель **Создание учетной записи хранения**.
4. В поле **Имя** введите имя учетной записи хранения, например *contoso369b2c*. Это значение будет позже называться *storageAccountName*.
5. Выберите соответствующую ценовую категорию, группу ресурсов и подписку. Убедитесь, что флажок **Закрепить на начальной панели** установлен. Нажмите кнопку **Создать**.
6. Вернитесь на начальную панель и щелкните созданную учетную запись хранения.
7. В разделе **Службы** щелкните **BLOB-объекты**. Откроется панель **Служба BLOB-объектов**.
8. Щелкните **+ Container** (+ Контейнер).
9. В поле **Имя** ведите имя для контейнера, например *b2c*. Это значение будет позже называться *containerName*.
9. Задайте для параметра **Тип доступа** значение **BLOB-объект**. Нажмите кнопку **Создать**.
10. Созданный контейнер появится в списке на панели **Служба BLOB-объектов**.
11. Закройте панель **Большие двоичные объекты**.
12. На панели **учетной записи хранения** щелкните значок **Ключ**. Откроется панель **Ключи доступа**.  
13. Запишите значение **key1**. Далее это значение называется *key1*.

## <a name="downloading-the-helper-tool"></a>Скачивание вспомогательного средства

1.  Скачайте вспомогательное средство с [GitHub](https://github.com/azureadquickstarts/b2c-azureblobstorage-client/archive/master.zip).
2.  Сохраните файл *B2C-AzureBlobStorage-Client-master.zip* на локальный компьютер.
3.  Извлеките содержимое файла B2C-AzureBlobStorage-Client-master.zip на локальный диск, например в папку **UI-Customization-Pack**. В ней появится папка *B2C-AzureBlobStorage-Client-master*.
4.  Откройте эту папку и извлеките содержимое архива *B2CAzureStorageClient.zip*.

## <a name="upload-the-ui-customization-pack-sample-files"></a>Передача примеров файлов UI-Customization-Pack

1.  С помощью проводника Windows перейдите в папку *B2C-AzureBlobStorage-Client-master*, расположенную в папке *UI-Customization-Pack*, созданной при работе с предыдущим разделом.
2.  Запустите файл *B2CAzureStorageClient.exe*. Эта программа загружает все файлы в каталог, указанный в вашей учетной записи хранения, и обеспечивает доступ к этим файлам с помощью CORS.
3.  При появлении запроса укажите:  Имя учетной записи хранения *storageAccountName*, например *contoso369b2c*.
    2\.  Первичный ключ доступа хранилища BLOB-объектов Azure *key1*, например *contoso369b2c*.
    c.  Имя контейнера хранилища BLOB-объектов *containerName*, например *b2c*.
    d.  Путь к примерам файлов *Starter-Pack*, например *..\B2CTemplates\wingtiptoys*.

Если вы следовали предыдущим действиям, то файлы HTML5 и CSS *UI-Customization-Pack* вымышленной компании **wingtiptoys** теперь указывают на вашу учетную запись хранения.  Чтобы проверить, правильно ли передано содержимое, откройте панель связанного контейнера на портале Azure. Кроме того, это можно проверить на странице браузера. Дополнительные сведения см. в статье [ Руководство по настройке пользовательского интерфейса приложений в Azure Active Directory B2C](active-directory-b2c-reference-ui-customization-helper-tool.md).

## <a name="ensure-the-storage-account-has-cors-enabled"></a>Проверка включения механизма CORS в учетной записи хранения

CORS необходимо включить в конечной точке, чтобы экземпляр Azure AD B2C мог загрузить содержимое. Это необходимо, так как содержимое размещается в домене, отличном от домена, из которого Azure AD B2C будет обслуживать страницу.

Чтобы убедиться, что в хранилище, в котором размещается содержимое, включен механизм CORS, сделайте следующее:

1. Откройте браузер и перейдите на страницу *unified.html*, используя полный URL-адрес расположения в вашей учетной записи хранения `https://<storageAccountName>.blob.core.windows.net/<containerName>/unified.html`. Например, https://contoso369b2c.blob.core.windows.net/b2c/unified.html.
2. Перейдите на страницу https://test-cors.org. Этот сайт позволяет убедиться, что на используемой странице включен механизм CORS.  
   <!--
   ![test-cors.org](../../media/active-directory-b2c-customize-ui-of-a-user-journey/test-cors.png)
   -->

3. В поле **Remote URL** (Удаленный URL-адрес) введите полный URL-адрес содержимого unified.html и щелкните **Отправить запрос**.
4. Убедитесь, что выходные данные в разделе **Результаты** содержат *XHR status: 200*. Это означает, что CORS включен.
   <!--
   ![CORS enabled](../../media/active-directory-b2c-customize-ui-of-a-user-journey/cors-enabled.png)
   -->
   Теперь учетная запись хранения должна содержать контейнер больших двоичных объектов с именем *b2c* со следующими шаблонами wingtiptoys из *Starter-Pack*.

<!--
![Correctly configured storage account](../../articles/active-directory-b2c/media/active-directory-b2c-reference-customize-ui-custom/storage-account-final.png)
-->

В следующей таблице описано назначение предыдущих страниц HTML5.

| Шаблон HTML5 | Описание |
|----------------|-------------|
| *phonefactor.html* | Эта страница используется как шаблон для страницы Многофакторной идентификации. |
| *ResetPassword.html* | Эту страницу можно использовать в качестве шаблона для страницы восстановления пароля. |
| *selfasserted.html* | Эта страница используется как шаблон для страницы регистрации учетных записей социальных сетей, страницы регистрации локальной учетной записи или страницы входа в локальную учетную запись. |
| *unified.html* | Эта страница используется как шаблон для единой страницы регистрации или входа в систему. |
| *updateprofile.html* | Эта страница используется как шаблон для страницы обновления профиля. |

## <a name="add-a-link-to-your-html5css-templates-to-your-user-journey"></a>Добавление ссылки на шаблоны HTML5 и CSS к пути взаимодействия пользователя

Вы можете добавить ссылку на шаблоны HTML5 и CSS к пути взаимодействия пользователя, изменив пользовательскую политику напрямую.

Пользовательские шаблоны HTML5 и CSS для использования в пути взаимодействия пользователя нужно указать в списке определений содержимого, которое можно использовать. Для этой цели, необязательный  *\<ContentDefinitions >* XML должен быть объявлен в разделе  *\<BuildingBlocks >* раздел XML-файла пользовательской политики.

В следующей таблице описан набор идентификаторов определений содержимого, распознаваемых подсистемой идентификации Azure AD B2C, и типы страниц, относящихся к ним.

| Идентификатор для определения содержимого | Описание |
|-----------------------|-------------|
| *api.error* | **Страница ошибки.** Эта страница отображается при обнаружении исключения или ошибки. |
| *api.idpselections* | **Страница выбора поставщика удостоверений.** На этой странице содержится список поставщиков удостоверений, которых можно выбирать во время входа пользователя. Это поставщики удостоверений организаций, социальных сетей, например Facebook и Google+, или локальных учетных записей (адрес электронной почты или имя пользователя). |
| *api.idpselections.signup* | **Выбор поставщика удостоверений для регистрации.** На этой странице содержится список поставщиков удостоверений, которых можно выбирать во время регистрации. Это поставщики удостоверений организаций, социальных сетей, например Facebook и Google+, или локальных учетных записей (адрес электронной почты или имя пользователя). |
| *api.localaccountpasswordreset* | **Страница восстановления пароля.** Эта страница содержит форму, которую должен заполнить пользователь для выполнения сброса пароля.  |
| *api.localaccountsignin* | **Страница входа в локальную учетную запись.** Эта страница содержит форму, которую пользователь должен заполнить при входе в локальную учетную запись (на основе адреса электронной почты или имени пользователя). Форма может содержать текстовое поле ввода и окно ввода пароля. |
| *api.localaccountsignup* | **Страница регистрации локальной учетной записи.** Эта страница содержит форму, которую пользователь должен заполнить при регистрации локальной учетной записи (на основе адреса электронной почты или имени пользователя). Форма может содержать различные элементы управления для ввода текста, например: текстовое поле, поле ввода пароля, переключатель, раскрывающийся список с единственным выбором или флажки множественного выбора. |
| *api.phonefactor* | **Страница Многофакторной идентификации.** Эта страница позволяет пользователю подтвердить свой номер телефона (с помощью SMS-сообщения или голосового звонка) во время регистрации или входа. |
| *api.selfasserted* | **Страница регистрации учетной записи социальных сетей.** Эта страница содержит форму регистрации, которую нужно заполнить пользователю при регистрации с использованием существующей учетной записи от таких поставщиков удостоверений социальных сетей, как Facebook или Google+. Эта страница аналогична предыдущей странице регистрации локальных учетных записей, за исключением полей для ввода пароля. |
| *api.selfasserted.profileupdate* | **Страница обновления профиля.** Эта страница содержит форму, которую пользователь может использовать для обновления профиля. Эта страница аналогична предыдущей странице регистрации локальных учетных записей, за исключением полей для ввода пароля. |
| *api.signuporsignin* | **Единая страница регистрации или входа.**  На этой странице обрабатывается регистрация и вход пользователей, которые могут использовать поставщики удостоверений организаций, поставщики удостоверений социальных сетей, такие как Facebook или Google+, или локальные учетные записи.

## <a name="next-steps"></a>Дальнейшие действия
[Общие сведения о пользовательских политиках начального пакета Azure Active Directory B2C](active-directory-b2c-reference-custom-policies-understanding-contents.md)
