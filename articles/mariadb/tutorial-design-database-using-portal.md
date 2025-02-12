---
title: Руководство по Проектирование Базы данных Azure для MariaDB с помощью портала Azure
description: Это руководство содержит сведения о создании базы данных и сервера Базы данных Azure для MariaDB и управлении ими с помощью портала Azure.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: tutorial
ms.date: 04/15/2019
ms.custom: mvc
ms.openlocfilehash: 1938a84d3e9c0ba8c84cbdbd2eee5b7ca448554d
ms.sourcegitcommit: adb6c981eba06f3b258b697251d7f87489a5da33
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/04/2019
ms.locfileid: "66515663"
---
# <a name="tutorial-design-an-azure-database-for-mariadb-database-by-using-the-azure-portal"></a>Руководство по Проектирование базы данных в службе "База данных Azure для MariaDB" с помощью портала Azure

База данных Azure для MariaDB — это управляемая служба, которую можно использовать для запуска, администрирования и масштабирования высокодоступных баз данных MySQL в облаке. С помощью портала Azure можно легко управлять сервером и проектировать базы данных.

Из этого руководства вы узнаете, как с помощью портала Azure выполнять следующие операции:

> [!div class="checklist"]
> * создание Базы данных Azure для MariaDB;
> * настройка брандмауэра сервера;
> * использование программы командной строки MySQL для создания базы данных;
> * Загрузка примера данных
> * Запрос данных
> * Обновление данных
> * восстановление данных.

Если у вас еще нет подписки Azure, создайте [бесплатную учетную запись](https://azure.microsoft.com/free/) Azure, прежде чем начинать работу.

## <a name="sign-in-to-the-azure-portal"></a>Вход на портал Azure

В браузере перейдите на [портал Azure](https://portal.azure.com/). Введите свои учетные данные для входа на портал. Панель мониторинга службы является представлением по умолчанию.

## <a name="create-an-azure-database-for-mariadb-server"></a>Создание сервера Базы данных Azure для MariaDB

Сервер Базы данных Azure для MariaDB создается с определенным набором [вычислительных ресурсов и ресурсов хранения](concepts-pricing-tiers.md). Он создается в [группе ресурсов Azure](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview).

1. Нажмите кнопку **Создать ресурс** (+) в левом верхнем углу окна портала.

2. Выберите **Базы данных** > **База данных Azure для MariaDB**. Чтобы найти службу, вы также можете ввести в поле поиска **MariaDB**.
   
   ![Переход к MySQL](./media/tutorial-design-database-using-portal/1-Navigate-to-mariadb.png)

3. Щелкните плитку **База данных Azure для MariaDB**, а затем нажмите кнопку **Создать**. Введите или выберите необходимые сведения.
   
   ![Создание формы](./media/tutorial-design-database-using-portal/2-create-form.png)

    Параметр | Рекомендуемое значение | Описание поля 
    ---|---|---
    Имя сервера | *уникальное имя сервера* | Выберите уникальное имя, идентифицирующее сервер Базы данных Azure для MariaDB. Например, **mydemoserver**. Имя домена *mariadb.database.azure.com* добавляется к введенному имени сервера. Имя сервера может содержать только строчные буквы, цифры и знак дефиса (-). Оно должно содержать от 3 до 63 символов.
    Подписка | *ваша подписка* | Выберите подписку Azure, которую вы хотите использовать для сервера. Если у вас есть несколько подписок, выберите ту, в которой взимается плата за использование ресурса.
    Группа ресурсов | **myresourcegroup** | Введите имя новой группы ресурсов или выберите имеющуюся.
    Выбор источника | **Пустой** | Чтобы создать сервер, выберите **Пустой**. (Выберите **Архивация** при создании сервера из геоизбыточного хранилища резервных копий существующего сервера Базы данных Azure для MariaDB.)
    учетные данные администратора сервера для входа; | **myadmin** | Учетная запись входа, используемая при подключении к серверу. Не используйте для имени учетной записи администратора такие варианты: **azure_superuser**, **admin**, **administrator**, **root**, **guest** или **public**.
    Пароль | *по своему выбору* | Введите новый пароль учетной записи администратора сервера. Пароль должен содержать от 8 до 128 символов. Пароль должен содержать символы из таких трех категорий: прописные латинские буквы, строчные латинские буквы, цифры (0–9) и небуквенно-цифровые знаки (!, $, #, % и т. д.).
    Подтверждение пароля. | *по своему выбору*| Подтвердите пароль учетной записи администратора.
    Расположение | *ближайший к пользователям регион*| Выберите расположение, наиболее близкое к пользователям или другим приложениям Azure.
    Version (версия) | *последняя версия*| Последняя версия (если нет особых требований использовать другую версию).
    Ценовая категория | См. описание. | Конфигурации вычислительных ресурсов, хранилища и резервного копирования для нового сервера. Выберите **Ценовая категория** > **Общего назначения**. Оставьте значения по умолчанию для следующих параметров:<br><ul><li>**Поколение вычислительных ресурсов** (поколение 5);</li><li>**Виртуальное ядро** (4 виртуальных ядра);</li><li>**Хранилище** (100 ГБ);</li><li>**Срок хранения резервных копий** (7 дней).</li></ul><br>Чтобы включить резервное копирование сервера в геоизбыточном хранилище, выберите **Геоизбыточное хранилище** в разделе **Варианты резервирования для архивации**. <br><br>Щелкните **ОК**, чтобы сохранить ценовую категорию. На следующем снимке экрана показаны выбранные параметры.
    
   ![Ценовая категория](./media/tutorial-design-database-using-portal/3-pricing-tier.png)

   > [!TIP]
   > Функция **автоматического увеличения** позволяет серверу нарастить объем хранения при приближении к предельному значению без последствий для рабочей нагрузки.

4. Нажмите кнопку **Создать**. Через несколько минут в облаке будет создан сервер Базы данных Azure для MariaDB. На панели инструментов щелкните **Уведомления**, чтобы отслеживать процесс развертывания.

## <a name="configure-the-firewall"></a>Настройка брандмауэра

База данных Azure для MariaDB защищена брандмауэром. По умолчанию все подключения к серверу и базам данных на сервере отклоняются. Перед первым подключением к Базе данных Azure для MariaDB настройте брандмауэр, чтобы добавить IP-адрес общедоступной сети клиентского компьютера (или диапазон IP-адресов).

1. Выберите созданный сервер, а затем щелкните **Безопасность подключения**.
   
   ![Безопасность подключения](./media/tutorial-design-database-using-portal/1-Connection-security.png)
2. Вы можете выбрать **Добавить мой IP-адрес** или настроить правила брандмауэра. Не забудьте нажать кнопку **Сохранить** после создания правил.

Теперь вы можете подключиться к серверу с помощью программы командной строки MySQL или с помощью инструмента MySQL Workbench.

> [!TIP]
> Сервер Базы данных Azure для MariaDB обменивается данными через порт 3306. При попытке подключиться из корпоративной сети исходящий трафик через порт 3306 должен блокироваться сетевым брандмауэром. В этом случае, чтобы подключиться к серверу Базы данных SQL Azure для MariaDB, ваш ИТ-отдел должен открыть порт 3306.

## <a name="get-connection-information"></a>Получение сведений о подключении

Получите значения для полей **Имя сервера** (полное имя) и **Имя для входа администратора сервера** для сервера Базы данных Azure для MariaDB на портале Azure. Полное имя сервера используется для подключения к серверу с помощью программы командной строки MySQL. 

1. На [портале Azure](https://portal.azure.com/) в меню слева щелкните **Все ресурсы**. Введите имя сервера и найдите сервер Базы данных Azure для MariaDB. Выберите имя сервера, чтобы просмотреть сведения о сервере.

2. Запишите значения **Имя сервера** и **Имя для входа администратора сервера** на странице **Обзор**. Вы также можете нажать кнопку **Копировать** рядом с каждым полем, чтобы скопировать его значение в буфер обмена.

   ![Свойства сервера](./media/tutorial-design-database-using-portal/2-server-properties.png)

В нашем примере серверу присвоено имя **mydemoserver.mariadb.database.azure.com**, а имя для входа администратора сервера — **myadmin\@mydemoserver**.

## <a name="connect-to-the-server-by-using-mysql"></a>Подключение к серверу с помощью MySQL

Используйте [программу командной строки MySQL](https://dev.mysql.com/doc/refman/5.7/en/mysql.html), чтобы подключиться к серверу Базы данных Azure для MariaDB. Можно запустить программу командной строки MySQL из Azure Cloud Shell в браузере или на собственном компьютере, используя локальные инструменты MySQL. Чтобы открыть Azure Cloud Shell, нажмите кнопку **Попробовать** в блоке кода в этой статье или перейдите на портал Azure и щелкните значок **>_** на панели инструментов справа вверху. 

Введите команду для подключения:

```azurecli-interactive
mysql -h mydemoserver.mariadb.database.azure.com -u myadmin@mydemoserver -p
```

## <a name="create-a-blank-database"></a>Создание пустой базы данных

Подключившись к серверу, создайте пустую базу данных для работы:

```sql
CREATE DATABASE mysampledb;
```

Выполните следующую команду в командной строке, чтобы подключиться к созданной базе данных:

```sql
USE mysampledb;
```

## <a name="create-tables-in-the-database"></a>Создание таблиц в базе данных

Теперь, когда вы знаете, как подключиться к Базе данных Azure для MariaDB, можно выполнить некоторые основные задачи.

Сначала создайте таблицу и заполните ее некоторыми данными. Давайте создадим таблицу, в которой хранятся данные инвентаризации:

```sql
CREATE TABLE inventory (
    id serial PRIMARY KEY, 
    name VARCHAR(50), 
    quantity INTEGER
);
```

## <a name="load-data-in-the-tables"></a>Загрузка данных в таблицы

Теперь, когда таблица создана, мы можем вставить в нее данные. Чтобы вставить некоторые строки данных, в открытом окне командной строки выполните следующий запрос:

```sql
INSERT INTO inventory (id, name, quantity) VALUES (1, 'banana', 150); 
INSERT INTO inventory (id, name, quantity) VALUES (2, 'orange', 154);
```

## <a name="query-and-update-the-data-in-the-tables"></a>Запрос и обновление данных в таблицах

Чтобы извлечь сведения из таблицы базы данных, выполните приведенный ниже запрос:

```sql
SELECT * FROM inventory;
```

Вы можете также обновить данные в таблицах, выполнив следующую команду:

```sql
UPDATE inventory SET quantity = 200 WHERE name = 'banana';
```

Строка обновится при извлечении данных:

```sql
SELECT * FROM inventory;
```

## <a name="restore-a-database-to-a-previous-point-in-time"></a>Восстановление базы данных до предыдущей точки во времени

Предположим, вы случайно удалили важную таблицу из базы данных и вам не удается легко восстановить данные. База данных Azure для MariaDB позволяет восстановить сервер до точки во времени, создавая копию баз данных на новом сервере. Вы можете восстановить удаленные данные с помощью нового сервера. Указанные ниже шаги позволяют восстановить сервер до точки во времени, когда еще не была создана таблица.

1. На портале Azure найдите Базу данных Azure для MariaDB. На странице **Обзор** выберите **Восстановить**.

   ![Восстановление базы данных](./media/tutorial-design-database-using-portal/1-restore-a-db.png)

2. На странице **восстановления** введите или выберите следующие сведения.
   
   ![Форма восстановления](./media/tutorial-design-database-using-portal/2-restore-form.png)
   
   - **Точка восстановления.** Выберите точку во времени, до которой требуется восстановить данные, в пределах указанного периода времени. Обязательно преобразуйте свое местное время в формат UTC.
   - **Восстановить на новом сервере**. Введите имя нового сервера, на который нужно восстановить данные.
   - **Расположение.** Регион совпадает с исходным сервером и не может быть изменен.
   - **Ценовая категория**. Ценовая категория совпадает с исходным сервером и не может быть изменена.
   
3. Чтобы [восстановить сервер до точки во времени](./howto-restore-server-portal.md) перед удалением таблицы, нажмите кнопку **OК**. Восстановление сервера приведет к созданию новой копии сервера на выбранный момент времени. 

## <a name="next-steps"></a>Дополнительная информация
Из этого руководства вы узнали, как с помощью портала Azure выполнять следующие операции:

> [!div class="checklist"]
> * создание Базы данных Azure для MariaDB;
> * настройка брандмауэра сервера;
> * использование программы командной строки MySQL для создания базы данных;
> * Загрузка примера данных
> * Запрос данных
> * Обновление данных
> * восстановление данных.

> [!div class="nextstepaction"]
> [Подключение приложений к Базе данных Azure для MariaDB](./howto-connection-string.md)
