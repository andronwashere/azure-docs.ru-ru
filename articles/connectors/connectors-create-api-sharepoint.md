---
title: Подключение к SharePoint из Azure Logic Apps | Документация Майкрософт
description: Автоматизация задач и рабочих процессов, которые отслеживают ресурсы и управляют ими в SharePoint Online или локальном экземпляре SharePoint Server с помощью Azure Logic Apps
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.assetid: e0ec3149-507a-409d-8e7b-d5fbded006ce
ms.topic: article
tags: connectors
ms.date: 08/25/2018
ms.openlocfilehash: e636b2bb08477e6c56c6ae41f08983fc5bfa2a9b
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "60450749"
---
# <a name="monitor-and-manage-sharepoint-resources-with-azure-logic-apps"></a>Мониторинг ресурсов SharePoint и управление ими с помощью Azure Logic Apps

С помощью Azure Logic Apps и соединителя SharePoint вы можете создавать автоматизированные задачи и рабочие процессы, которые отслеживают ресурсы (или управляют ими), такие как файлы, папки, списки, элементы, пользователи и т. д., в SharePoint Online или в локальном экземпляре SharePoint Server, например:

* Мониторинг создания, изменения либо удаления файлов или элементов.
* Создание, получение, обновление или удаление элементов.
* Добавление, получение или удаление вложений. Получение содержимого из вложений.
* Создание, копирование, обновление или удаление файлов. 
* Обновление свойств файла. Получение содержимого, метаданных или свойств для файла.
* Создание списка или извлечение папок.
* Получение списков или представлений списка.
* Указание состояния утверждения содержимого.
* Разрешение пользователей.
* Отправление HTTP-запросов в SharePoint.
* Получение значений сущности.

Вы можете использовать триггеры, которые получают ответы из SharePoint и делают выходные данные доступными для других действий. Вы можете использовать действия в своих приложениях логики для выполнения задач в SharePoint. Кроме того, выходные данные от действий SharePoint могут использоваться другими действиями. Например, если вы регулярно получаете файлы из SharePoint, вы можете отправлять сообщения своей команде с помощью соединителя Slack.
Если вы не знакомы с приложениями логики, ознакомьтесь со статьей [Что такое Azure Logic Apps](../logic-apps/logic-apps-overview.md).

## <a name="prerequisites"></a>Технические условия

* Подписка Azure. Если у вас еще нет подписки Azure, <a href="https://azure.microsoft.com/free/" target="_blank">зарегистрируйтесь для получения бесплатной учетной записи Azure</a>. 

* Адрес сайта SharePoint и учетные данные пользователя

  Ваши учетные данные авторизуют приложение логики, чтобы оно могло создать подключение и получить доступ к вашей учетной записи SharePoint. 

* Прежде чем подключать приложение логики к таким локальным системам, как SharePoint Server, нужно [установить и настроить локальный шлюз данных](../logic-apps/logic-apps-gateway-install.md). Таким образом, вы можете указать использовать установку шлюза при создании соединения SharePoint Server для вашего приложения логики.

* Базовые знания [создания приложений логики](../logic-apps/quickstart-create-first-logic-app-workflow.md).

* Приложение логики, из которого необходимо получить доступ к учетной записи SharePoint. Чтобы запускать действия с помощью триггера SharePoint, [создайте пустое приложение логики](../logic-apps/quickstart-create-first-logic-app-workflow.md). Чтобы использовать действие SharePoint, настройте запуск своего приложения логики с помощью триггера, например триггера Salesforce, если у вас есть учетная запись Salesforce.

  Например, вы можете запускать свое приложение логики с помощью триггера Salesforce **When a record is created** (При создании записи). 
  Этот триггер срабатывает каждый раз, когда в Salesforce создается новая запись, например о потенциальном клиенте. 
  Затем вы можете выполнить этот триггер с действием **Создание файла** в SharePoint. Таким образом, при создании новой записи ваше приложение логики создает файл в SharePoint с информацией об этой новой записи.

## <a name="connect-to-sharepoint"></a>Подключение к SharePoint

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

1. Войдите на [портал Azure](https://portal.azure.com) и откройте свое приложение логики в конструкторе приложений логики, если оно еще не открыто.

1. Для пустого приложения логики в поле поиска в качестве фильтра введите sharepoint. В списке триггеров выберите нужный триггер. 

   -или-

   В имеющихся приложениях логики на последнем шаге, в котором необходимо добавить действие SharePoint, выберите **Новый шаг**. 
   В поле поиска введите sharepoint в качестве фильтра. 
   В списке действий выберите любое необходимое действие.

   Чтобы добавить действие между шагами, переместите указатель на стрелку между шагами. 
   Выберите появившийся знак "плюс" ( **+** ), а затем щелкните **Добавить действие**.

1. Когда вам будет предложено войти в систему, укажите необходимые сведения о подключении. Если вы используете SharePoint Server, убедитесь, что выбран **подключиться через локальный шлюз данных**. Когда все будет готово, выберите **Создать**.

1. Укажите необходимые сведения для выбранного триггера или действия и продолжайте создание рабочего процесса приложения логики.

## <a name="connector-reference"></a>Справочник по соединителям

Дополнительные технические сведения о триггерах, действиях и ограничениях, которые приведены в описании OpenAPI соединителя (прежнее название — Swagger), можно найти на [странице справки](/connectors/sharepoint/) соединителя.

## <a name="get-support"></a>Получение поддержки

* Если у вас возникли вопросы, то посетите [форум Azure Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).
* Отправить идею по поводу возможности или проголосовать за нее вы можете на [сайте отзывов пользователей Logic Apps](https://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Дальнейшие действия

* См. дополнительные сведения о других [соединителях Logic Apps](../connectors/apis-list.md).
