---
title: Публикация управляемого приложения Azure на портале | Документация Майкрософт
description: Создание с помощью портала Azure управляемого приложения Azure, предназначенного для членов вашей организации.
services: managed-applications
author: tfitzmac
manager: timlt
ms.service: managed-applications
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.date: 11/02/2017
ms.author: tomfitz
ms.openlocfilehash: f27d30d4709fbf373c8458629d0c8c5af4333acf
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "61297094"
---
# <a name="publish-a-service-catalog-application-through-azure-portal"></a>Публикация приложения каталога службы на портале Azure

С помощью портала Azure вы можете публиковать [управляемые приложения Azure](overview.md), предназначенные для членов вашей организации. Например, отдел ИТ может публиковать управляемые приложения, которые обеспечивают соответствие стандартам организации. Эти управляемые приложения доступны в каталоге служб, а не в Azure Marketplace.

## <a name="prerequisites"></a>Технические условия

При публикации управляемого приложения необходимо указать удостоверение для управления ресурсами. Мы рекомендуем указать группу пользователей Azure Active Directory См. дополнительные сведения о [создании группы пользователей и добавлении в нее пользователей в Azure Active Directory](../active-directory/fundamentals/active-directory-groups-create-azure-portal.md). 

ZIP-файл, содержащий определение управляемого приложения, должен быть доступным при использовании URI. Мы рекомендуем отправить ZIP-файл в хранилище BLOB-объектов. 

## <a name="create-managed-application-with-portal"></a>Создание управляемого приложения с помощью портала

1. В левом верхнем углу выберите **+Создать**.

   ![Новая служба](./media/publish-portal/new.png)

1. Выполните поиск словосочетания **каталог услуг**.

1. В результатах найдите **Определение управляемого приложения каталога услуг**. Выберите его.

   ![Поиск определений управляемых приложений](./media/publish-portal/select-managed-apps-definition.png)

1. Выберите **Создать**, чтобы создать определение управляемого приложения.

   ![Создание определения управляемого приложения](./media/publish-portal/create-definition.png)

1. Укажите значения имени, отображаемого имени, описания, расположения, подписки и группы ресурсов. Для URI файла пакета укажите путь к созданному вами ZIP-файлу.

   ![Определение значений](./media/publish-portal/fill-application-values.png)

1. Когда откроется раздел "Аутентификация и уровень блокировки", выберите **Добавить авторизацию**.

   ![Добавление авторизации](./media/publish-portal/add-authorization.png)

1. Выберите группу пользователей Azure Active Directory для управления ресурсами и нажмите **ОК**.

   ![Добавление группы авторизации](./media/publish-portal/add-auth-group.png)

1. Указав все значения, выберите **Создать**.

   ![Создание управляемого приложения](./media/publish-portal/create-app.png)

## <a name="next-steps"></a>Дальнейшие действия

* Общие сведения об управляемых приложениях Azure см. в разделе [Обзор управляемых приложений Azure](overview.md).
* См. дополнительные сведения о [примерах проектов для управляемых приложений Azure](sample-projects.md).
* Чтобы узнать, как создать файл определения пользовательского интерфейса для управляемого приложения, изучите статью [Начало работы с CreateUiDefinition](create-uidefinition-overview.md).