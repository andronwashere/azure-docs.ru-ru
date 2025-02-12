---
title: Развертывание приложения из каталога служб с помощью портала Azure| Документация Майкрософт
description: Показывает потребителям Управляемых приложений, как развернуть приложение из каталога служб с помощью портала Azure.
services: managed-applications
author: tfitzmac
ms.service: managed-applications
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.date: 10/04/2018
ms.author: tomfitz
ms.openlocfilehash: 4d2e8b442f70ee791fe65a32402e5272eda3f209
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "60588908"
---
# <a name="deploy-service-catalog-app-through-azure-portal"></a>Развертывание приложения из каталога служб с помощью портала Azure

В предыдущем руководстве [Публикация определения для управляемого приложения Azure](publish-managed-app-definition-quickstart.md) вы опубликовали определение для управляемого приложения. В этом кратком руководстве вы создадите приложение из каталога служб из этого определения.

## <a name="create-service-catalog-app"></a>Создание приложения из каталога служб

На портале Azure выполните указанные ниже действия.

1. Выберите **Создать ресурс**.

   ![Создание ресурса](./media/deploy-service-catalog-quickstart/create-new.png)

1. Найдите **Управляемое приложение каталога услуг** и выберите его в списке доступных вариантов.

   ![Поиск приложения в каталоге служб](./media/deploy-service-catalog-quickstart/select-service-catalog.png)

1. Вы видите описание службы управляемого приложения. Нажмите кнопку **Создать**.

   ![Выбор кнопки "Создать"](./media/deploy-service-catalog-quickstart/create-service-catalog.png)

1. На портале показаны определения управляемого приложения, к которым у вас есть доступ. Из доступных определений выберите то, которое вы хотите развернуть. В этом кратком руководстве используйте определение **Управляемая учетная запись**, которое вы создали в предыдущем кратком руководстве. Нажмите кнопку **Создать**.

   ![Выбор определения для развертывания](./media/deploy-service-catalog-quickstart/select-definition.png)

1. Укажите значения для вкладки **Основы**. Выберите подписку Azure, в которой вы хотите развернуть приложение из каталога служб. Создайте новую группу ресурсов с именем **applicationGroup**. Выберите расположение для вашего приложения. По завершении выберите **ОК**.

   ![Укажите значения для основы](./media/deploy-service-catalog-quickstart/provide-basics.png)

1. Укажите префикс для имени учетной записи хранения. Выберите тип учетной записи хранения, который нужно создать. По завершении выберите **ОК**.

   ![Укажите значения для хранилища](./media/deploy-service-catalog-quickstart/provide-storage.png)

1. Проверьте сводные данные. Если проверка прошла успешно, нажмите кнопку **ОК**, чтобы начать развертывание.

   ![Просмотр итогов](./media/deploy-service-catalog-quickstart/view-summary.png)

## <a name="view-results"></a>Просмотр результатов

После развертывания приложения из каталога служб у вас будет две новые группы ресурсов. Одна группа ресурсов содержит приложение из каталога служб. Другая группа ресурсов содержит ресурсы для приложения из каталога служб.

1. Просмотрите группу ресурсов с именем **applicationGroup**, чтобы увидеть приложение из каталога служб.

   ![Просмотр приложения](./media/deploy-service-catalog-quickstart/view-managed-application.png)

1. Просмотрите группу ресурсов с именем **applicationGroup{hash-characters}** , чтобы увидеть ресурсы для приложения из каталога служб.

   ![Просмотр ресурсов](./media/deploy-service-catalog-quickstart/view-resources.png)

## <a name="next-steps"></a>Дальнейшие действия

* Чтобы узнать, как создать файлы определений для управляемого приложения, см. статью [Создание и публикация определения управляемого приложения](publish-service-catalog-app.md).
* Для Azure CLI см. статью [Развертывание управляемого приложения для каталога службы с помощью Azure CLI](./scripts/managed-application-cli-sample-create-application.md).
* Для PowerShell см. статью [Развертывание управляемого приложения для каталога служб с помощью Azure PowerShell](./scripts/managed-application-poweshell-sample-create-application.md).