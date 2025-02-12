---
title: Создание реплик чтения и управление ими в базе данных Azure для MySQL
description: В этой статье описывается, как создать реплики чтения и управлять ими в базе данных Azure для MySQL с помощью портала.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 04/29/2019
ms.openlocfilehash: b422718a1eaec483acdc2c8ab37442b9aea78aaa
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "65510766"
---
# <a name="how-to-create-and-manage-read-replicas-in-azure-database-for-mysql-using-the-azure-portal"></a>Создание реплик чтения и управление ими в базе данных Azure для MySQL с помощью портала Azure

В этой статье вы узнаете, как создавать и управлять ими чтения реплик в базе данных Azure для службы MySQL с помощью портала Azure.

> [!IMPORTANT]
> Можно создать чтения реплики, в том же регионе, что главный сервер, или в любом другом регионе Azure по своему усмотрению. Репликация между регионами в настоящее время находится в общедоступной предварительной версии.

## <a name="prerequisites"></a>Технические условия

- [Сервер базы данных Azure для MySQL](quickstart-create-mysql-server-database-using-azure-portal.md), который будет использоваться в качестве главного сервера.

> [!IMPORTANT]
> Функция создания реплики чтения доступна только для серверов базы данных Azure для MySQL в ценовой категории "Общее назначение" или "Оптимизированная для операций в памяти". Убедитесь, что главный сервер находится в одной из этих ценовых категорий.

## <a name="create-a-read-replica"></a>Создание реплики чтения

Чтобы создать сервер-реплику чтения, выполните следующие действия.

1. Войдите на [портал Azure](https://portal.azure.com/).

2. Выберите имеющийся сервер базы данных Azure для MySQL, который будет главным сервером. Откроется страница **Обзор**.

3. В меню в разделе **Параметры** выберите **Репликация**.

4. Выберите **Добавить реплику**.

   ![База данных Azure для MySQL — репликация](./media/howto-read-replica-portal/add-replica.png)

5. Введите имя сервера-реплики.

    ![База данных Azure для MySQL: имя реплики](./media/howto-read-replica-portal/replica-name.png)

6. Выберите расположение для сервера-реплики. Можно создать реплику в любом регионе Azure. Расположение по умолчанию совпадает с главного сервера

    ![База данных Azure для MySQL — расположение реплики](./media/howto-read-replica-portal/replica-location.png)

7. Выберите **ОК** для подтверждения создания реплики.

> [!NOTE]
> Реплики чтения создаются с той же конфигурацией сервера, что и у главного сервера. Вы можете изменить созданную конфигурацию сервера-реплики. Чтобы сервер-реплика мог работать с главным сервером, рекомендуется, чтобы значения конфигурации сервера-реплики были равны или превосходили значения конфигурации главного сервера.

Созданный сервер-реплику можно просмотреть в колонке **Репликация**.

   ![База данных Azure для MySQL — список реплик](./media/howto-read-replica-portal/list-replica.png)

## <a name="stop-replication-to-a-replica-server"></a>Остановка репликации на сервер-реплику

> [!IMPORTANT]
> Остановка репликации на сервер является необратимой операцией. Репликацию, остановленную между главным сервером и сервером-репликой, отменить невозможно. Сервер-реплика становится автономным и начинает поддерживает операции чтения и записи. Это сервер нельзя снова преобразовать в реплику.

Чтобы остановить репликацию между главным сервером и сервером-репликой на портале Azure, следуйте инструкциям ниже.

1. На портале Azure выберите нужный главный сервер базы данных Azure для MySQL. 

2. В меню в разделе **Параметры** выберите **Репликация**.

3. Выберите сервер-реплику, для которого нужно остановить репликацию.

   ![База данных Azure для MySQL — выбор сервера, для которого нужно остановить репликацию](./media/howto-read-replica-portal/stop-replication-select.png)

4. Щелкните **Остановить репликацию**.

   ![База данных Azure для MySQL — остановка репликации](./media/howto-read-replica-portal/stop-replication.png)

5. Подтвердите остановку репликации, нажав кнопку **ОК**.

   ![База данных Azure для MySQL — подтверждение остановки репликации](./media/howto-read-replica-portal/stop-replication-confirm.png)

## <a name="delete-a-replica-server"></a>Удаление сервера-реплики

Чтобы удалить сервер-реплику чтения на портале Azure, следуйте инструкциям ниже.

1. На портале Azure выберите нужный главный сервер базы данных Azure для MySQL.

2. В меню в разделе **Параметры** выберите **Репликация**.

3. Выберите сервер-реплику, который нужно удалить.

   ![База данных Azure для MySQL — выбор удаляемого сервера-реплики](./media/howto-read-replica-portal/delete-replica-select.png)

4. Щелкните **Удалить реплику**.

   ![База данных Azure для MySQL — удаление реплики](./media/howto-read-replica-portal/delete-replica.png)

5. Введите имя реплики и нажмите кнопку **Удалить**, чтобы подтвердить удаление реплики.  

   ![База данных Azure для MySQL — подтверждение удаления реплики](./media/howto-read-replica-portal/delete-replica-confirm.png)

## <a name="delete-a-master-server"></a>Удаление главного сервера

> [!IMPORTANT]
> Удаление главного сервера приводит к остановке репликации на все серверы-реплики и удалению самого главного сервера. Серверы-реплики становятся автономными серверами, которые начинают поддерживать операции чтения и записи.

Чтобы удалить главный сервер на портале Azure, следуйте инструкциям ниже.

1. На портале Azure выберите нужный главный сервер базы данных Azure для MySQL.

2. На странице **Обзор** выберите **Удалить**.

   ![База данных Azure для MySQL — удаление главного сервера](./media/howto-read-replica-portal/delete-master-overview.png)

3. Введите имя главного сервера и нажмите кнопку **Удалить**, чтобы подтвердить его удаление  

   ![База данных Azure для MySQL — удаление главного сервера](./media/howto-read-replica-portal/delete-master-confirm.png)

## <a name="monitor-replication"></a>Мониторинг репликации

1. На [портале Azure](https://portal.azure.com/) выберите сервер-реплику базы данных Azure для MySQL, который нужно отследить.

2. В разделе боковой панели **Мониторинг** выберите **Метрики**.

3. В раскрывающемся списке доступных метрик выберите **Replication lag in seconds** (Задержка репликации в секундах).

   ![Выбор задержки репликации](./media/howto-read-replica-portal/monitor-select-replication-lag.png)

4. Выберите нужный диапазон времени. На рисунке ниже выбран диапазон в 30 минут.

   ![Выбор диапазона времени](./media/howto-read-replica-portal/monitor-replication-lag-time-range.png)

5. Просмотрите задержку репликации для выбранного диапазона времени. На рисунке ниже отображаются последние 30 минут.

   ![Выбор диапазона времени](./media/howto-read-replica-portal/monitor-replication-lag-time-range-thirty-mins.png)

## <a name="next-steps"></a>Дальнейшие действия

- Узнайте больше о [репликах чтения](concepts-read-replicas.md)