---
title: Настройка PostgreSQL на виртуальной машине Linux | Документация Майкрософт
description: Узнайте, как установить и настроить PostgreSQL на виртуальной машине Linux в Azure.
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: gwallace
editor: ''
tags: azure-resource-manager,azure-service-management
ms.assetid: 1a747363-0cc5-4ba3-9be7-084dfeb04651
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 02/01/2016
ms.author: cynthn
ms.openlocfilehash: 086b36b347f214e1e9cdf44e4fb5a29fe501fa8b
ms.sourcegitcommit: 2e4b99023ecaf2ea3d6d3604da068d04682a8c2d
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/09/2019
ms.locfileid: "67667110"
---
# <a name="install-and-configure-postgresql-on-azure"></a>Установка и настройка PostgreSQL в Azure
PostgreSQL — это расширенная открытая СУБД, аналогичная СУБД Oracle и DB2. Она предлагает возможности корпоративного уровня, обеспечивая полное соответствие принципам ACID, надежную обработку транзакций и управление параллелизмом в разных версиях. Она также поддерживает такие стандарты, как ANSI SQL и SQL/MED (включая оболочки для внешних данных Oracle, MySQL, MongoDB и др.). Высокая расширяемость обеспечивается поддержкой более 12 процедурных языков, индексов GIN и GIST, пространственных данных, различных функций NoSQL для JSON и приложений на основе пары "ключ — значение".

Из этой статьи вы узнаете, как установить и настроить СУБД PostgreSQL на виртуальной машине Azure под управлением Linux.

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="install-postgresql"></a>Установка PostgreSQL
> [!NOTE]
> Для выполнения инструкций учебника у вас уже должна быть виртуальная машина Microsoft Azure под управлением Linux. Прежде чем продолжить, создайте и настройте виртуальную машину под управлением Linux, используя сведения, приведенные в [руководстве по виртуальным машинам Azure под управлением Linux](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
> 
> 

В качестве порта PostgreSQL используйте порт 1999.  

Подключитесь к виртуальной машине под управлением Linux, используя утилиту PuTTY. Если виртуальная машина Linux Azure используется впервые, изучите статью [Использование SSH с Linux в Azure](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), чтобы узнать, как использовать PuTTY для подключения к виртуальной машине Linux.

1. Выполните следующую команду, чтобы переключиться на привилегированного пользователя (администратора):
   
        # sudo su -
2. Для некоторых дистрибутивов требуется перед установкой PostgreSQL установить другие программы. Найдите свой дистрибутив в списке и выполните соответствующую команду:
   
   * Linux на базе Red Hat:
     
           # yum install readline-devel gcc make zlib-devel openssl openssl-devel libxml2-devel pam-devel pam  libxslt-devel tcl-devel python-devel -y  
   * Linux на базе Debian:
     
            # apt-get install readline-devel gcc make zlib-devel openssl openssl-devel libxml2-devel pam-devel pam libxslt-devel tcl-devel python-devel -y  
   * SUSE Linux:
     
           # zypper install readline-devel gcc make zlib-devel openssl openssl-devel libxml2-devel pam-devel pam  libxslt-devel tcl-devel python-devel -y  
3. Загрузите PostgreSQL в корневой каталог и распакуйте пакет:
   
        # wget https://ftp.postgresql.org/pub/source/v9.3.5/postgresql-9.3.5.tar.bz2 -P /root/
   
        # tar jxvf  postgresql-9.3.5.tar.bz2
   
    Выше приведен лишь пример. Более точный адрес для скачивания см. в [индексе каталога /pub/source/](https://ftp.postgresql.org/pub/source/).
4. Чтобы запустить сборку, выполните следующие команды:
   
        # cd postgresql-9.3.5
   
        # ./configure --prefix=/opt/postgresql-9.3.5
5. Если вы хотите включить в сборку все возможные компоненты, в том числе документацию (HTML и руководства) и дополнительные модули (от участников сообщества), выполните следующую команду вместо предыдущих:
   
        # gmake install-world
   
    Должно появиться следующее подтверждение:
   
        PostgreSQL, contrib, and documentation successfully made. Ready to install.

## <a name="configure-postgresql"></a>Настройка PostgreSQL
1. (Необязательно) Создайте символьную ссылку на PostgreSQL, исключив номер версии:
   
        # ln -s /opt/postgresql-9.3.5 /opt/pgsql
2. Создайте каталог для базы данных:
   
        # mkdir -p /opt/pgsql_data
3. Создайте непривилегированного пользователя и измените его профиль. Затем переключитесь на этого пользователя (в нашем примере — *postgres* ):
   
        # useradd postgres
   
        # chown -R postgres.postgres /opt/pgsql_data
   
        # su - postgres
   
   > [!NOTE]
   > В целях безопасности для инициализации, запуска или завершения работы базы данных в PostgreSQL используется непривилегированный пользователь.
   > 
   > 
4. Измените файл *bash_profile*, используя следующие команды. Эти строки будут добавлены в конец файла *bash_profile*:
   
        cat >> ~/.bash_profile <<EOF
        export PGPORT=1999
        export PGDATA=/opt/pgsql_data
        export LANG=en_US.utf8
        export PGHOME=/opt/pgsql
        export PATH=\$PATH:\$PGHOME/bin
        export MANPATH=\$MANPATH:\$PGHOME/share/man
        export DATA=`date +"%Y%m%d%H%M"`
        export PGUSER=postgres
        alias rm='rm -i'
        alias ll='ls -lh'
        EOF
5. Запустите файл *bash_profile*:
   
        $ source .bash_profile
6. Проверьте установку, используя следующую команду:
   
        $ which psql
   
    Если установка выполнена успешно, появится такой ответ:
   
        /opt/pgsql/bin/psql
7. Вы также можете проверить версию PostgreSQL:
   
        $ psql -V

8. Инициализируйте базу данных:
   
        $ initdb -D $PGDATA -E UTF8 --locale=C -U postgres -W
   
    Вы должны увидеть следующий результат:

![image](./media/postgresql-install/no1.png)

## <a name="set-up-postgresql"></a>Настройка PostgreSQL
<!--    [postgres@ test ~]$ exit -->

Выполните следующие команды:

    # cd /root/postgresql-9.3.5/contrib/start-scripts

    # cp linux /etc/init.d/postgresql

Измените две переменные в файле /etc/init.d/postgresql. В качестве префикса задайте путь установки PostgreSQL: **/opt/pgsql**. В качестве значения PGDATA задайте путь к хранилищу данных из PostgreSQL: **/opt/pgsql_data**.

    # sed -i '32s#usr/local#opt#' /etc/init.d/postgresql

    # sed -i '35s#usr/local/pgsql/data#opt/pgsql_data#' /etc/init.d/postgresql

![image](./media/postgresql-install/no2.png)

Сделайте файл исполняемым:

    # chmod +x /etc/init.d/postgresql

Запустите PostgreSQL:

    # /etc/init.d/postgresql start

Убедитесь, что конечная точка PostgreSQL включена:

    # netstat -tunlp|grep 1999

Вы должны увидеть следующий результат.

![image](./media/postgresql-install/no3.png)

## <a name="connect-to-the-postgres-database"></a>Подключение к базе данных Postgres
Снова переключитесь на пользователя postgres:

    # su - postgres

Создайте базу данных Postgres:

    $ createdb events

Подключитесь к только что созданной базе данных events:

    $ psql -d events

## <a name="create-and-delete-a-postgres-table"></a>Создание и удаление таблицы Postgres
После подключения к базе данных можно приступить к созданию таблиц в ней.

Например, можно создать новую таблицу Postgres с помощью следующей команды:

    CREATE TABLE potluck (name VARCHAR(20),    food VARCHAR(30),    confirmed CHAR(1), signup_date DATE);

Теперь у нас есть таблица из четырех столбцов со следующими именами и ограничениями:

1. Столбец name (имя) не может быть длиннее 20 символов (ограничение команды VARCHAR).
2. Столбец food (еда) указывает блюдо, которое принесет каждый пользователь. Команда VARCHAR устанавливает ограничение в 30 символов.
3. В столбце confirmed (подтверждено) указывается, подтвердил ли человек свое участие. Допустимые значения: «Y» (да) и «N» (нет).
4. В столбце date (дата) будет отображаться дата регистрации на мероприятие. В Postgres даты должны записываться в формате гггг-мм-дд.

Если таблица создана успешно, вы должны увидеть следующее:

![image](./media/postgresql-install/no4.png)

Вы также можете проверить структуру таблицы, используя следующую команду:

![image](./media/postgresql-install/no5.png)

### <a name="add-data-to-a-table"></a>Добавление данных в таблицу
Прежде всего, вставьте в строку следующие сведения:

    INSERT INTO potluck (name, food, confirmed, signup_date) VALUES('John', 'Casserole', 'Y', '2012-04-11');

Вы должны увидеть такой результат:

![image](./media/postgresql-install/no6.png)

Вы можете добавить в таблицу еще пару человек. Здесь мы привели несколько примеров, однако вы можете создать собственные:

    INSERT INTO potluck (name, food, confirmed, signup_date) VALUES('Sandy', 'Key Lime Tarts', 'N', '2012-04-14');

    INSERT INTO potluck (name, food, confirmed, signup_date) VALUES ('Tom', 'BBQ','Y', '2012-04-18');

    INSERT INTO potluck (name, food, confirmed, signup_date) VALUES('Tina', 'Salad', 'Y', '2012-04-18');

### <a name="show-tables"></a>Просмотр таблиц
Чтобы отобразить таблицу, используйте следующую команду:

    select * from potluck;

Результаты:

![image](./media/postgresql-install/no7.png)

### <a name="delete-data-in-a-table"></a>Удаление данных из таблицы
Для удаления данных из таблицы используйте следующую команду:

    delete from potluck where name=’John’;

Она удалит все данные в строке John. Результаты:

![image](./media/postgresql-install/no8.png)

### <a name="update-data-in-a-table"></a>Обновление данных в таблице
Для обновления данных в таблице используйте следующую команду: В нашем Sandy подтверждает, что они конференции, поэтому мы изменим RSVP из «N» на «Y»:

     UPDATE potluck set confirmed = 'Y' WHERE name = 'Sandy';


## <a name="get-more-information-about-postgresql"></a>Получение дополнительных сведений о PostgreSQL
Завершив установку PostgreSQL на виртуальную машину Azure под управлением Linux, вы сможете использовать ее в Azure. Дополнительные сведения о PostgreSQL см. на [веб-сайте PostgreSQL](https://www.postgresql.org/).

