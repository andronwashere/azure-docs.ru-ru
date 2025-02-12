---
title: создание рабочей области Студии машинного обучения;
titleSuffix: Azure Machine Learning Studio
description: Для использования Студии машинного обучения Azure требуется рабочая область Студии машинного обучения. Такая рабочая область содержит инструменты, необходимые для создания, публикации экспериментов и управления ими.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: xiaoharper
ms.author: amlstudiodocs
ms.custom: seodec18
ms.date: 12/07/2017
ms.openlocfilehash: 7aeee4f24f6c7133ad978bc0c6c7fb8853bc4c35
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "62109354"
---
# <a name="create-and-share-an-azure-machine-learning-studio-workspace"></a>Создание рабочей области Студии машинного обучения Azure и предоставление к ней общего доступа

Для использования Студии машинного обучения Azure требуется рабочая область Студии машинного обучения. Такая рабочая область содержит инструменты, необходимые для создания, публикации экспериментов и управления ими.

## <a name="create-a-studio-workspace"></a>Создание рабочей области Студии

1. Войдите на [портал Azure](https://portal.azure.com/)

    > [!NOTE]
    > Чтобы войти и создать рабочую область, требуются права администратора подписки Azure. 
    >
    > 

2. Щелкните **+Создать**.

3. В поле поиска введите **рабочая область студии машинного обучения** и выберите соответствующий элемент. Щелкните **Создать** в нижней части страницы.

4. Введите сведения о рабочей области:

   - *Имя рабочей области* может содержать до 260 символов и не должно заканчиваться пробелом. Оно не может содержать следующие символы: `< > * % & : \ ? + /`.
   - Выбранный (или созданный) *план веб-службы* и связанная *ценовая категория* используются при развертывании веб-служб из этой рабочей области.

     ![Создание новой рабочей области Студии](./media/create-workspace/create-new-workspace.png)

5. Нажмите кнопку **Создать**.

> [!NOTE]
> Студия машинного обучения использует учетную запись хранения Azure, предоставленную пользователем, для хранения промежуточных данных при выполнении рабочего процесса. После создания рабочей области, в случае удаления учетной записи хранения или изменения ключей доступа, эта рабочая область перестанет действовать и все эксперименты в ней завершатся сбоем.
Если вы случайно удалите учетную запись хранения, вы можете восстановить ее, создав повторно такую же (с тем же именем и в том же регионе) и повторно синхронизировать ключ доступа. Если вы изменили ключи доступа к учетной записи хранения, синхронизируйте их повторно в рабочей области, используя портал Azure.

После развертывания рабочей области вы можете открыть Студию машинного обучения.

1. Перейдите в Студию машинного обучения, щелкнув ссылку [https://studio.azureml.net/](https://studio.azureml.net/).

2. В правом верхнем углу выберите рабочую область.

    ![Выбор рабочей области](./media/create-workspace/open-workspace.png)

3. Щелкните **my experiments** (Мои эксперименты).

    ![Открытие экспериментов](./media/create-workspace/my-experiments.png)

Сведения об управлении рабочей областью Студии см. в статье [Управление рабочей областью Студии машинного обучения Azure](manage-workspace.md).
Если возникла проблема при создании рабочей области, обратитесь к разделу [Руководство по устранению неполадок. Создание рабочей области Студии машинного обучения Azure и подключение к ней](troubleshooting-creating-ml-workspace.md).


## <a name="share-an-azure-machine-learning-studio-workspace"></a>Предоставление общего доступа к рабочей области Студии машинного обучения Azure
После создания рабочей области Студии машинного обучения вы можете приглашать в нее пользователей, предоставляя доступ к области и всем ее экспериментам, наборам данных, записным книжкам и т. д. Вы можете добавлять пользователей со следующими ролями:

* **Пользователь.** Пользователь рабочей области может создавать, открывать, изменять и удалять наборы данных, эксперименты и т. д. в рабочей области.
* **Владелец.** Владелец может приглашать и удалять пользователей рабочей области, а также управлять доступными пользователям действиями.

> [!NOTE]
> Учетная запись администратора, создающего рабочую область, автоматически добавляется в качестве владельца. Но другим администраторам или пользователям в этой подписке автоматически не предоставляется доступ к рабочей области. Их необходимо пригласить явным образом.
> 
> 

### <a name="to-share-a-studio-workspace"></a>Предоставление общего доступа к рабочей области Студии

1. Войдите в Студию машинного обучения, щелкнув ссылку [https://studio.azureml.net/Home](https://studio.azureml.net/Home).

2. На панели слева щелкните **Параметры**.

3. Откройте вкладку **Пользователи**.

4. В нижней части страницы щелкните **Invite more users** (Пригласить больше пользователей).

    ![Параметры Студии](./media/create-workspace/settings.png)

5. Укажите один или несколько адресов электронной почты. У пользователя должна быть действующая учетная запись Майкрософт или учетная запись организации (из Azure Active Directory).

6. Выберите, с какими правами вы хотите добавить пользователей: владельца или пользователя.

7. Щелкните значок флажка **ОК**.

Каждый добавленный пользователь получит сообщение по электронной почте с инструкциями по входу в общую рабочую область.

> [!NOTE]
> Чтобы пользователи могли развертывать веб-службы и управлять ими в этой рабочей области, они должны быть участниками или администраторами подписки Azure. 



