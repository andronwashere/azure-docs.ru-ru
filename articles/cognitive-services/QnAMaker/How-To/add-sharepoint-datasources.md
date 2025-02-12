---
title: Файлы SharePoint — QnA Maker
titleSuffix: Azure Cognitive Services
description: Добавьте защищенных источников данных SharePoint в базу знаний для обогащения знаний с вопросами и ответами, которые могут быть защищены с помощью Active Directory.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 06/24/2019
ms.author: diberry
ms.openlocfilehash: ecb9777643296685d0dcc7cd5a177f2fe00d2580
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/09/2019
ms.locfileid: "67704629"
---
# <a name="add-a-secured-sharepoint-data-source-to-your-knowledge-base"></a>Добавление знаний защищенного источника данных SharePoint

Добавьте защищенных источников данных SharePoint в базу знаний для обогащения знаний с вопросами и ответами, которые могут быть защищены с помощью Active Directory. 

При добавлении конфиденциальный документ SharePoint знаний, как диспетчер QnA Maker, необходимо запросить разрешение Active Directory для QnA Maker. Это разрешение предоставляется из Active Directory диспетчера QnA Maker для доступа к SharePoint, он не должен быть предоставлен снова. Каждый последующих добавления в базу знаний не потребуется авторизации, если он находится в один и тот же ресурс SharePoint. 

Если диспетчер знаний QnA Maker не manager Active Directory, необходимо связаться с диспетчером Active Directory, чтобы завершить этот процесс.

## <a name="add-supported-file-types-to-knowledge-base"></a>Добавить поддерживаемые типы файлов для базы знаний

Вы можете добавить все поддерживаемые QnA Maker [типы файлов](../Concepts/data-sources-supported.md) с сайта SharePoint в базу знаний. Может потребоваться предоставить [разрешения](#permissions) Если защищенный файл ресурсов.

1. Из библиотеки с сайтом SharePoint, выберите кнопку с многоточием меню файла, `...`.
1. Скопируйте URL-адрес файла.

   ![Получите URL-адрес SharePoint файла, выбрав меню с многоточием файла, затем копирование URL-адрес.](../media/add-sharepoint-datasources/get-sharepoint-file-url.png)

1. На портале QnA Maker на **параметры** странице [добавить URL-адрес](edit-knowledge-base.md#add-datasource) в базу знаний. 

### <a name="images-with-sharepoint-files"></a>Образы с файлами SharePoint

Если файлы включают в себя образы, их не извлекаются. Образ, можно добавить на портале QnA Maker, после файла на пары QnA.

Добавьте изображение со следующим синтаксисом markdown: 

```markdown
![Explanation or description of image](URL of public image)
```

Текст в квадратные скобки, `[]`, описание образа. URL-адрес в круглые скобки, `()`, имеет прямую ссылку на изображение. 

При тестировании пары QnA панели интерактивных тестов, на портале QnA Maker, отображается изображение вместо текста markdown. Это подтверждает изображения могут быть получены публично из клиентского приложения.

## <a name="permissions"></a>Разрешения

Предоставление разрешений происходит при добавлении защищенного файла с сервера SharePoint базы знаний. В зависимости от настроек SharePoint вверх и разрешения пользователя, добавив файл, это может потребовать:

* никаких дополнительных действий — пользователь, добавляющий файл имеет все необходимые разрешения.
* действия по [знаний manager](#knowledge-base-manager-add-sharepoint-data-source-in-qna-maker-portal) и [manager Active Directory](#active-directory-manager-grant-file-read-access-to-qna-maker).

См. приведенные ниже действия. 

### <a name="knowledge-base-manager-add-sharepoint-data-source-in-qna-maker-portal"></a>Диспетчер базы знаний: Добавление источника данных SharePoint в портал QnA Maker

Когда **manager QnA Maker** добавляет конфиденциальный документ SharePoint с базой знаний, manager инициирует знаний запрос для разрешения, которое должен выполнить диспетчер Active Directory.

Запрос начинается с всплывающее окно для проверки подлинности с учетной записью Active Directory. 

![Проверки подлинности учетной записи пользователя](../media/add-sharepoint-datasources/authenticate-user-account.png)

Когда диспетчер QnA Maker выбирает учетную запись, Azure Active Directory администратор будет получать Обратите внимание, что им необходимо разрешить доступ к ресурсу SharePoint приложений (не диспетчер QnA Maker) QnA Maker. Диспетчер Azure Active Directory должен сделать это для каждого ресурса SharePoint, но не каждый документ в этот ресурс. 

### <a name="active-directory-manager-grant-file-read-access-to-qna-maker"></a>Диспетчер Active directory: предоставлять доступ на чтение файла QnA Maker

Диспетчер Active Directory (не диспетчер QnA Maker) необходимо предоставить доступ к QnA Maker, доступ к ресурсу SharePoint, выбрав [эту ссылку](https://login.microsoftonline.com/common/oauth2/v2.0/authorize?response_type=id_token&scope=Files.Read%20Files.Read.All%20Sites.Read.All%20User.Read%20User.ReadBasic.All%20profile%20openid%20email&client_id=c2c11949-e9bb-4035-bda8-59542eb907a6&redirect_uri=https%3A%2F%2Fwww.qnamaker.ai%3A%2FCreate&state=68) Чтобы авторизовать приложение QnA Maker портала SharePoint enterprise для чтения файлов разрешения. 

![Диспетчер Azure Active Directory предоставляет разрешение в интерактивном режиме](../media/add-sharepoint-datasources/aad-manager-grants-permission-interactively.png)

<!--
The Active Directory manager must grant QnA Maker access either by application name, `QnAMakerPortalSharePoint`, or by application ID, `c2c11949-e9bb-4035-bda8-59542eb907a6`. 
-->
<!--
### Grant access from the interactive pop-up window 

The Active Directory manager will get a pop-up window requesting permissions to the `QnAMakerPortalSharePoint` app. The pop-up window includes the QnA Maker Manager email address that initiated the request, an `App Info` link to learn more about **QnAMakerPortalSharePoint**, and a list of permissions requested. Select **Accept** to provide those permissions. 

![Azure Active Directory manager grants permission interactively](../media/add-sharepoint-datasources/aad-manager-grants-permission-interactively.png)
-->
<!--

### Grant access from the App Registrations list

1. The Active Directory manager signs in to the Azure portal and opens **[App registrations list](https://ms.portal.azure.com/#blade/Microsoft_AAD_IAM/ApplicationsListBlade)**. 

1. Search for and select the **QnAMakerPortalSharePoint** app. Change the second filter box from **My apps** to **All apps**. The app information will open on the right side.

    ![Select QnA Maker app in App registrations list](../media/add-sharepoint-datasources/select-qna-maker-app-in-app-registrations.png)

1. Select **Settings**.

    [![Select Settings in the right-side blade](../media/add-sharepoint-datasources/select-settings-for-qna-maker-app-registration.png)](../media/add-sharepoint-datasources/select-settings-for-qna-maker-app-registration.png#lightbox)

1. Under **API access**, select **Required permissions**. 

    ![Select 'Settings', then under 'API access', select 'Required permission'](../media/add-sharepoint-datasources/select-required-permissions-in-settings-blade.png)

1. Do not change any settings in the **Enable Access** window. Select **Grant Permission**. 

    [![Under 'Grant Permission', select 'Yes'](../media/add-sharepoint-datasources/grant-app-required-permissions.png)](../media/add-sharepoint-datasources/grant-app-required-permissions.png#lightbox)

1. Select **YES** in the pop-up confirmation windows. 

    ![Grant required permissions](../media/add-sharepoint-datasources/grant-required-permissions.png)
-->
### <a name="grant-access-from-the-azure-active-directory-admin-center"></a>Предоставление доступа в центре администрирования Azure Active Directory

1. Диспетчер Active Directory выполняет вход на портал Azure и открывает  **[корпоративные приложения](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/StartboardApplicationsMenuBlade/AllApps)** . 

1. Поиск `QnAMakerPortalSharePoint` выберите нужное приложение QnA Maker. 

    [![Поиск QnAMakerPortalSharePoint в списке корпоративных приложений](../media/add-sharepoint-datasources/search-enterprise-apps-for-qna-maker.png)](../media/add-sharepoint-datasources/search-enterprise-apps-for-qna-maker.png#lightbox)

1. В разделе **безопасности**, перейдите в меню **разрешения**. Выберите **предоставления согласия администратора организации**. 

    [![Выберите пользователя, прошедшего проверку подлинности для администратора Active Directory](../media/add-sharepoint-datasources/grant-aad-permissions-to-enterprise-app.png)](../media/add-sharepoint-datasources/grant-aad-permissions-to-enterprise-app.png#lightbox)

1. Выберите учетную запись единого входа с разрешениями на предоставление разрешений для Active Directory. 


  
<!--

## Add SharePoint data source with APIs

You need to get the SharePoint file's URI before adding it to QnA Maker. 

## Get SharePoint File URI

Use the following steps to transform the SharePoint URL into a sharing token.

1. Encode the URL using [base64](https://en.wikipedia.org/wiki/Base64). 

1. Convert the base64-encoded result to an unpadded base64url format with the following character changes. 

    * Remove the equal character, `=` from the end of the value. 
    * Replace `/` with `_`. 
    * Replace `+` with `-`. 
    * Append `u!` to be beginning of the string. 

1. Sign in to Graph explorer and run the following query, where `sharedURL` is ...:

    ```
    https://graph.microsoft.com/v1.0/shares/<sharedURL>/driveitem
    ```

    Get the **@microsoft.graph.downloadUrl** and use this as `fileuri` in the QnA Maker APIs.

### Add or update a SharePoint File URI to your knowledge base

Use the **@microsoft.graph.downloadUrl** from the previous section as the `fileuri` in the QnA Maker API for [adding a knowledge base](https://go.microsoft.com/fwlink/?linkid=2092179) or [updating a knowledge base](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase/update). The following fields are mandatory: name, fileuri, filename, source.

```
{
    "name": "Knowledge base name",
    "files": [
        {
            "fileUri": "<@microsoft.graph.downloadURL>",
            "fileName": "filename.xlsx",
            "source": "<SharePoint link>"
        }
    ],
    "urls": [],
    "users": [],
    "hostUrl": "",
    "qnaList": []
}
```



## Remove QnA Maker app from SharePoint authorization

1. Use the steps in the previous section to find the Qna Maker app in the Active Directory admin center. 
1. When you select the **QnAMakerPortalSharePoint**, select **Overview**. 
1. Select **Delete** to remove permissions. 

-->

## <a name="next-steps"></a>Дальнейшие действия

> [!div class="nextstepaction"]
> [Совместная работа над вашей базы знаний](collaborate-knowledge-base.md)
