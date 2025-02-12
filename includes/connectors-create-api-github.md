---
title: включение файла
description: включение файла
services: logic-apps
author: MandiOhlinger
ms.service: logic-apps
ms.topic: include
ms.date: 03/02/2018
ms.author: mandia
ms.custom: include file
ms.openlocfilehash: 11280e1678f52ede928cb2a85ea83add222e15fa
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/18/2019
ms.locfileid: "67185550"
---
1. Создайте пустое приложение логики на [портале Azure](https://portal.azure.com). 

2. В конструкторе Logic Apps введите "github" в качестве фильтра. 

3. Выберите соединитель и триггер GitHub, которые хотите использовать.

   ![Выбор соединителя и триггера GitHub](./media/connectors-create-api-github/github-connector.png)

   > [!NOTE]
   > Все рабочие процессы приложений логики должны запускаться с помощью триггера. Действия можно выбрать, только если рабочий процесс приложения логики уже запущен при помощи триггера. 

4. Если вы еще не создали подключение, выберите **Войти** и при появлении запроса введите учетные данные GitHub.  

   ![Вход с помощью учетных данных GitHub](./media/connectors-create-api-github/github-connector-sign-in-credentials.png)

   Приложение логики использует эти учетные данные, чтобы авторизовать подключение и доступ к данным для вашей учетной записи GitHub. 

5. Укажите имя пользователя и пароль своей учетной записи GitHub, а затем подтвердите авторизацию.

   ![Указание учетных данных и подтверждение авторизации](./media/connectors-create-api-github/github-connector-authorize.png)   

   Подключение создано на портале Azure и готово к использованию.

6. Продолжайте определять рабочий процесс приложения логики.

   ![Добавление действий в рабочий процесс приложения логики](./media/connectors-create-api-github/github-connector-logic-app.png)

