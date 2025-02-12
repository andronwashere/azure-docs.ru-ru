---
title: включение файла
description: включение файла
services: cosmos-db
author: rimman
ms.service: cosmos-db
ms.topic: include
ms.date: 04/08/2019
ms.author: rimman
ms.custom: include file
ms.openlocfilehash: 5d57d7e18befba175a5a8a825494ce512644b5a2
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/26/2019
ms.locfileid: "67185040"
---
1. Войдите на [портале Azure](https://portal.azure.com/).
1. Последовательно выберите **Создать ресурс** > **Базы данных** > **Azure Cosmos DB**.
   
   ![Область "Базы данных" на портале Azure](./media/cosmos-db-create-dbaccount/create-nosql-db-databases-json-tutorial-1.png)

1. На странице **создания учетной записи Azure Cosmos DB** введите основные параметры для новой учетной записи Azure Cosmos. 
 
    |Параметр|Значение|ОПИСАНИЕ |
    |---|---|---|
    |Subscription|Имя подписки|Выберите подписку Azure, которую нужно использовать для этой учетной записи Azure Cosmos. |
    |Группа ресурсов|Имя группы ресурсов|Выберите группу ресурсов или **Создать**, затем введите уникальное имя для новой группы ресурсов. |
    | Имя учетной записи|Введите уникальное имя.|Введите имя для идентификации учетной записи Azure Cosmos. Так как элемент *documents.azure.com* добавляется к указанному вами идентификатору для создания URI, используйте уникальный идентификатор.<br><br>Идентификатор может содержать только строчные буквы, цифры и знак дефиса (-). Длина — от 3 до 31 знака.|
    | API|Core (SQL)|API определяет тип учетной записи, которую нужно создать. Azure Cosmos DB предоставляет пять API: Core (SQL) и MongoDB для данных документа, Gremlin для данных графа, таблица Azure и Cassandra. Сейчас для каждого API требуется создавать отдельную учетную запись. <br><br>Выберите **Core (SQL)** для создания базы данных документов и запроса с использованием синтаксиса SQL. <br><br>[Дополнительные сведения об API SQL](../articles/cosmos-db/documentdb-introduction.md)|
    | Location|Выберите ближайший к пользователям регион|Выберите географическое расположение для размещения учетной записи Azure Cosmos DB. Используйте ближайшее к пользователям расположение, чтобы предоставить им максимально быстрый доступ к данным.|
   
   ![Страница "Новая учетная запись" для Azure Cosmos DB](./media/cosmos-db-create-dbaccount/azure-cosmos-db-create-new-account.png)

1. Выберите **Review + create** (Просмотреть и создать). Вы можете пропустить разделы **Сеть** и **Теги**. 

1. Проверьте параметры учетной записи, а затем нажмите кнопку **Создать**. Создание учетной записи занимает несколько минут. Дождитесь, пока на странице портала появится сообщение **Развертывание выполнено**. 

    ![Область "Уведомления" на портале Azure](./media/cosmos-db-create-dbaccount/azure-cosmos-db-account-created.png)

1. Выберите **Перейти к ресурсу**, чтобы перейти на страницу учетной записи Azure Cosmos DB. 

    ![Страница учетной записи Azure Cosmos DB](./media/cosmos-db-create-dbaccount/azure-cosmos-db-account-created-2.png)
