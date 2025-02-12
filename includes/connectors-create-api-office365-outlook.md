---
author: ecfan
ms.service: logic-apps
ms.topic: include
ms.date: 11/03/2016
ms.author: estfan
ms.openlocfilehash: b216de0a5094066977467b2899567122d585fb7e
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/18/2019
ms.locfileid: "67185546"
---
#### <a name="prerequisites"></a>Технические условия
* Учетная запись Azure. Вы можете создать [бесплатную учетную запись](https://azure.microsoft.com/free).
* Учетная запись [Office 365](https://office365.com).  

Прежде чем использовать свою учетную запись Office 365 в приложении логики, необходимо авторизовать приложение логики для подключения к этой учетной записи. Это можно легко сделать из приложения логики на портале Azure.  

Авторизуйте приложение логики для подключения к учетной записи Office 365, выполнив приведенные ниже действия.

1. Создайте приложение логики. В конструкторе приложений логики в раскрывающемся списке выберите параметр **Показать API, управляемые Майкрософт**, а затем введите в поле поиска "office 365". Выберите один из триггеров или одно из действий.  
    ![Этап создания подключения Office 365](./media/connectors-create-api-office365-outlook/office365-sendemail.png)  
2. Если вы ранее не создавали подключения к Office 365, то вам будет предложено ввести учетные данные Office 365.  
    ![Этап создания подключения Office 365](./media/connectors-create-api-office365-outlook/office365-signin.png)  
3. Выберите **Войти** и введите имя пользователя и пароль. Выберите **Войти**.  
    ![Этап создания подключения Office 365](./media/connectors-create-api-office365-outlook/office365-usernamepassword.png)
   
    Эти учетные данные используются для авторизации приложения логики, чтобы оно могло подключиться и получить доступ к учетной записи Office 365. 
4. Обратите внимание, что было создано подключение. Теперь перейдите к другим действиям в приложении логики.   
    ![Этап создания подключения Office 365](./media/connectors-create-api-office365-outlook/office365-sendemailproperties.png)  

