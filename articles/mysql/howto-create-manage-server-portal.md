---
title: Создание сервера базы данных Azure для MySQL и управление им с помощью портала Azure
description: В этой статье описывается, как быстро создать сервер базы данных Azure для MySQL и управлять этим сервером с помощью портала Azure.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 02/28/2018
ms.openlocfilehash: 6d6f24475497382dd9e04d3335fb89d6f0bdd514
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "61459564"
---
# <a name="create-and-manage-azure-database-for-mysql-server-using-azure-portal"></a>Создание сервера базы данных Azure для MySQL и управление им с помощью портала Azure
В этом разделе описывается, как быстро создать сервер базы данных Azure для MySQL. Он также содержит сведения о том, как управлять сервером с помощью портала Azure. Управление сервером включает в себя просмотр сведений о сервере и базах данных, сброс пароля, масштабирование ресурсов и удаление сервера.

## <a name="log-in-to-the-azure-portal"></a>Войдите на портал Azure.
Войдите на [портал Azure](https://portal.azure.com).

## <a name="create-an-azure-database-for-mysql-server"></a>Создайте сервер базы данных Azure для MySQL.
Чтобы создать сервер службы "База данных Azure для MySQL" с именем mydemoserver, сделайте следующее.

1. Нажмите кнопку **Создать ресурс** в верхнем левом углу на портале Azure.

2. На странице "Создание" выберите **Базы данных**, а затем на странице "Базы данных" выберите **База данных Azure для MySQL**.

    > Сервер базы данных Azure для MySQL создается с определенным набором [вычислительных ресурсов и ресурсов хранения](./concepts-pricing-tiers.md). База данных создается в группе ресурсов Azure и на логическом сервере базы данных Azure для MySQL.

   ![create-new-server](./media/howto-create-manage-server-portal/create-new-server.png)

3. Заполните форму базы данных Azure для MySQL, указав следующую информацию.

    | **Поле формы** | **Описание поля** |
    |----------------|-----------------------|
    | *Имя сервера* | mydemoserver (глобально уникальное имя сервера) |
    | *Подписка* | mysubscription (выберите из раскрывающегося меню) |
    | *Группа ресурсов* | myresourcegroup (создайте новую группу ресурсов или используйте существующую) |
    | *Выбрать источник* | Blank (создайте пустой сервер MySQL) |
    | *Имя для входа администратора сервера* | myadmin (укажите имя учетной записи администратора) |
    | *Пароль* | задайте пароль учетной записи администратора |
    | *Подтверждение пароля.* | Подтвердите пароль учетной записи администратора |
    | *Location* | Юго-Восточная Азия (выберите "Северная Европа" или "Западная часть США") |
    | *Версия* | 5.7 (выберите версию сервера службы "База данных Azure для MySQL") |

4. Щелкните **Ценовая категория**, чтобы указать уровень производительности и уровень служб для нового сервера. Выберите вкладку **Общего назначения**. *Поколение 5*, *2 виртуальных ядра*, *5 ГБ*, и *7 дней* — это значения по умолчанию для параметров **Поколение вычислительных ресурсов**, **Виртуальное ядро**, **Хранилище** и **Срок хранения резервных копий**. Можно оставить эти ползунки как есть. Чтобы включить резервное копирование сервера в геоизбыточном хранилище, выберите **Геоизбыточное** в разделе **параметров избыточности резервного копирования**.

   ![create-server-pricing-tier](./media/howto-create-manage-server-portal/create-server-pricing-tier.png)

5. Нажмите кнопку **Создать**, чтобы подготовить сервер. Подготовка занимает несколько минут.

    > Установите флажок **Закрепить на панели мониторинга**, чтобы с легкостью отслеживать процесс развертывания.

## <a name="update-an-azure-database-for-mysql-server"></a>Обновление сервера базы данных Azure для MySQL
После подготовки нового сервера пользователю доступны несколько способов настройки существующего сервера, включая сброс пароля администратора и увеличение или уменьшение масштаба сервера путем изменения виртуального ядра или хранилища.

### <a name="change-the-administrator-user-password"></a>Изменение пароля администратора
1. В области **Обзор** для сервера нажмите кнопку **сброс пароля** для отображения окна сброса пароля.

   ![Обзор](./media/howto-create-manage-server-portal/overview.png)

2. Введите новый пароль и подтвердите его в окне, как показано ниже:

   ![Сброс пароля](./media/howto-create-manage-server-portal/reset-password.png)

3. Нажмите кнопку **ОК** , чтобы сохранить новый пароль.

### <a name="scale-vcores-updown"></a>Увеличение или уменьшение масштаба виртуальных ядер

1. Щелкните элемент **Ценовая категория**, расположенный в области **Параметры**.

2. Измените параметр **Виртуальное ядро**, переместив ползунок на нужное значение.

    ![Масштабирование вычислений](./media/howto-create-manage-server-portal/scale-compute.png)

3. Нажмите кнопку **OK**, чтобы сохранить изменения.

### <a name="scale-storage-up"></a>Увеличение масштаба хранилища

1. Щелкните элемент **Ценовая категория**, расположенный в области **Параметры**.

2. Измените параметр **Хранилище**, переместив ползунок на нужное значение.

    ![Масштабирование хранилища](./media/howto-create-manage-server-portal/scale-storage.png)

3. Нажмите кнопку **OK**, чтобы сохранить изменения.

## <a name="delete-an-azure-database-for-mysql-server"></a>Удаление сервера базы данных Azure для MySQL

1. В области **Обзор** сервера нажмите кнопку **Удалить**, чтобы открыть запрос на подтверждение удаления.

    ![delete](./media/howto-create-manage-server-portal/delete.png)

2. Введите имя сервера в поле ввода для дополнительного подтверждения.

    ![Подтверждение удаления](./media/howto-create-manage-server-portal/confirm.png)

3. Нажмите кнопку **Удалить**, чтобы подтвердить удаление сервера. Дождитесь появления сообщения "Сервер MySQL удален" в строке уведомлений.

## <a name="list-the-azure-database-for-mysql-databases"></a>Вывод списка баз данных Azure для MySQL
Прокрутите область **Обзор** сервера вниз до появления плитки базы данных, расположенной внизу. Все базы данных на сервере отображаются в таблице.

   ![show-databases](./media/howto-create-manage-server-portal/show-databases.png)

## <a name="show-details-of-an-azure-database-for-mysql-server"></a>Отображение сведений о сервере базы данных Azure для MySQL
Щелкните элемент **Свойства**, расположенный в области **Параметры**, для просмотра подробных сведений о сервере.

![properties](./media/howto-create-manage-server-portal/properties.png)

## <a name="next-steps"></a>Дальнейшие действия

[Краткое руководство по созданию сервера Базы данных Azure для MySQL с помощью портала Azure](./quickstart-create-mysql-server-database-using-azure-portal.md)