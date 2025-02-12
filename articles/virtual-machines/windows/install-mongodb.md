---
title: Установка MongoDB на виртуальной машине Windows в Azure | Документация Майкрософт
description: Узнайте, как установить MongoDB на виртуальную машину Azure под управлением Windows Server 2012 R2, созданную посредством модели развертывания с помощью Resource Manager.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: gwallace
editor: ''
ms.assetid: 53faf630-8da5-4955-8d0b-6e829bf30cba
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 12/15/2017
ms.author: cynthn
ms.openlocfilehash: 1436eadace2ff57bde9d67201d6b38d4aee8f523
ms.sourcegitcommit: dad277fbcfe0ed532b555298c9d6bc01fcaa94e2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/10/2019
ms.locfileid: "67722659"
---
# <a name="install-and-configure-mongodb-on-a-windows-vm-in-azure"></a>Установка и настройка базы данных MongoDB на виртуальной машине Windows в Azure
[MongoDB](https://www.mongodb.org) — это популярная высокопроизводительная база данных NoSQL с открытым кодом. В этой статье приведены инструкции по установке и настройке MongoDB на виртуальной машине Windows Server 2016 в Azure. [MongoDB можно также установить на виртуальной машине Linux в Azure](../linux/install-mongodb.md).

## <a name="prerequisites"></a>Предварительные требования
Прежде чем установить и настроить MongoDB, необходимо создать виртуальную машину и по возможности добавить в нее диск данных. С помощью приведенных ниже ссылок можно ознакомиться со статьями, в которых описывается создание виртуальной машины и добавление диска данных.

* Создайте виртуальную машину Windows на [портале Azure](quick-create-portal.md) или с помощью [PowerShell](quick-create-powershell.md).
* Подключите диск данных к виртуальной машине Windows Server на [портале Azure](attach-managed-disk-portal.md) или с помощью [PowerShell](attach-disk-ps.md).

Чтобы начать установку и настройку MongoDB, [выполните вход в виртуальную машину Windows Server](connect-logon.md) с помощью удаленного рабочего стола.

## <a name="install-mongodb"></a>Установка MongoDB
> [!IMPORTANT]
> Функции безопасности MongoDB, такие как проверка подлинности и привязка IP-адреса, не включены по умолчанию. Функции безопасности необходимо включить перед развертыванием MongoDB в рабочей среде. Чтобы узнать больше, ознакомьтесь с [системой безопасности и аутентификацией MongoDB](https://www.mongodb.org/display/DOCS/Security+and+Authentication).


1. После подключения к виртуальной машине с помощью удаленного рабочего стола откройте Internet Explorer на панели задач.
2. При первом запуске Internet Explorer выберите **Использовать рекомендуемые параметры безопасности, конфиденциальности и совместимости** и нажмите кнопку **ОК**.
3. Конфигурация усиленной безопасности Internet Explorer включена по умолчанию. Добавьте веб-сайт MongoDB в список разрешенных сайтов.
   
   * Щелкните значок **Сервис** в правом верхнем углу.
   * В окне **Свойства браузера** выберите вкладку **Безопасность** и щелкните значок **Надежные сайты**.
   * Нажмите кнопку **Сайты**. Добавьте *https://\*.mongodb.com* в список надежных сайтов и закройте диалоговое окно.
     
     ![Настройка параметров безопасности Internet Explorer](./media/install-mongodb/configure-internet-explorer-security.png)
4. Перейдите на страницу [MongoDB - Downloads](https://www.mongodb.com/downloads) (MongoDB. Загрузка) (https://www.mongodb.com/downloads).
5. При необходимости выберите выпуск **Community Server** и последний стабильный выпуск *64-разрядной версии Windows Server 2008 R2 или более поздней версии*. Чтобы скачать установщик, щелкните **DOWNLOAD (msi)** (Скачать (MSI-файл)).
   
    ![Скачивание установщика MongoDB](./media/install-mongodb/download-mongodb.png)
   
    После завершения скачивания запустите установщик.
6. Прочитайте и примите условия лицензионного соглашения. Когда появится запрос, выберите **Complete** (Завершить).
7. При желании можно также установить Compass — графический интерфейс для MongoDB.
8. На последнем экране щелкните **Install** (Установить).

## <a name="configure-the-vm-and-mongodb"></a>Настройка виртуальной машины и MongoDB
1. Установщик MongoDB не обновляет переменные пути. Если расположение MongoDB `bin` не указано в переменной пути, необходимо указывать полный путь при каждом использовании исполняемого файла MongoDB. Вот как можно добавить расположение в переменную пути.
   
   * Щелкните правой кнопкой мыши меню **Пуск** и выберите пункт **Система**.
   * Выберите **Дополнительные параметры системы**, а затем щелкните **Переменные среды**.
   * В разделе **Системные переменные** выберите **Путь** и нажмите кнопку **Изменить**.
     
     ![Настройка переменных PATH](./media/install-mongodb/configure-path-variables.png)
     
     Добавьте путь к папке MongoDB, `bin`. Приложение MongoDB обычно устанавливается в папку *C:\Program Files\MongoDB*. Проверьте путь установки на виртуальной машине. В следующем примере в переменную `PATH` добавляется расположение по умолчанию для установки MongoDB.
     
     ```
     ;C:\Program Files\MongoDB\Server\3.6\bin
     ```
     
     > [!NOTE]
     > Обязательно добавьте начальную точку с запятой (`;`), чтобы указать, что добавляется расположение в переменную `PATH`.

2. Создайте каталоги данных и журналов MongoDB на диске данных. В меню **Пуск** выберите **Командная строка**. В следующих примерах создаются каталоги на диске F.
   
    ```
    mkdir F:\MongoData
    mkdir F:\MongoLogs
    ```
3. Запустите экземпляр MongoDB с помощью приведенной команды, соответствующим образом изменив путь к каталогам данных и журналов.
   
    ```
    mongod --dbpath F:\MongoData\ --logpath F:\MongoLogs\mongolog.log
    ```
   
    Может пройти несколько минут, пока MongoDB выделит место для файлов журналов и начнет ожидать передачи данных. Все сообщения журнала будут направляться в файл *F:\MongoLogs\mongolog.log*, когда сервер `mongod.exe` запустится и выделит место для файлов журналов.
   
   > [!NOTE]
   > Фокус командной строки будет оставаться на этой задаче, пока выполняется экземпляр MongoDB. Не закрывайте окно командной строки, чтобы не прерывать выполнение MongoDB. Или установите MongoDB в качестве службы, как описано на следующем шаге.

4. Для более надежной работы MongoDB установите `mongod.exe` как службу. Создание службы означает, что вам не нужно будет оставлять окно командной строки открытым каждый раз, когда необходимо использовать MongoDB. Создайте службу, как описано ниже, указав пути к своим каталогам данных и журналов, соответственно.
   
    ```
    mongod --dbpath F:\MongoData\ --logpath F:\MongoLogs\mongolog.log --logappend  --install
    ```
   
    Приведенная выше команда создает службу MongoDB с описанием "Mongo DB". Кроме того, указаны следующие параметры.
   
   * Параметр `--dbpath` указывает расположение каталога данных.
   * Необходимо указать файл журнала с помощью параметра `--logpath`, так как у работающей службы нет командного окна для вывода данных.
   * Параметр `--logappend` указывает, что после перезапуска службы выходные данные будут добавляться в имеющийся файл журнала.
   
   Выполните следующую команду, чтобы запустить службу MongoDB.
   
    ```
    net start MongoDB
    ```
   
    Чтобы больше узнать о создании службы MongoDB, ознакомьтесь с [настройкой службы Windows для MongoDB](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-windows/#mongodb-as-a-windows-service).

## <a name="test-the-mongodb-instance"></a>Тестирование экземпляра MongoDB
Теперь, когда компонент MongoDB выполняется как один экземпляр или установлен в качестве службы, можно приступить к созданию и использованию баз данных. Чтобы запустить оболочку администрирования MongoDB, откройте еще одно окно командной строки из меню **Пуск** и введите приведенную ниже команду.

```
mongo  
```

Вы можете вывести список баз данных с помощью команды `db`. Вставьте некоторые данные следующим образом.

```
db.foo.insert( { a : 1 } )
```

Выполните поиск данных следующим образом.

```
db.foo.find()
```

Вы должны увидеть результат, аналогичный приведенному ниже.

```
{ "_id" : "ObjectId("57f6a86cee873a6232d74842"), "a" : 1 }
```

Завершите работу консоли `mongo` следующим образом.

```
exit
```

## <a name="configure-firewall-and-network-security-group-rules"></a>Настройка брандмауэра и правил группы безопасности сети
После установки и запуска MongoDB следует открыть порт в брандмауэре Windows для удаленного подключения к MongoDB. Чтобы создать правило входящего трафика, разрешающее передачу данных через TCP-порт 27017, откройте командную строку PowerShell для администрирования и введите следующую команду.

```powerahell
New-NetFirewallRule `
    -DisplayName "Allow MongoDB" `
    -Direction Inbound `
    -Protocol TCP `
    -LocalPort 27017 `
    -Action Allow
```

Можно также создать правило с помощью графического средства управления **Брандмауэр Windows в режиме повышенной безопасности**. Создайте правило входящего трафика, чтобы разрешить использовать TCP-порт 27017.

При необходимости создайте правило группы безопасности сети, чтобы разрешить доступ к MongoDB извне подсети в существующей виртуальной сети Azure. Правила группы безопасности сети можно создать с помощью [портала Azure](nsg-quickstart-portal.md) или [Azure PowerShell](nsg-quickstart-powershell.md). Как и в правилах брандмауэра Windows, разрешите использование TCP-порта 27017 для виртуального сетевого интерфейса виртуальной машины MongoDB.

> [!NOTE]
> MongoDB использует TCP-порт 27017 по умолчанию. Этот порт можно изменить с помощью параметра `--port` при запуске `mongod.exe` вручную или из службы. Если вы изменяете порт, не забудьте обновить правила брандмауэра Windows и группы безопасности сети, созданные на предыдущих шагах.


## <a name="next-steps"></a>Следующие шаги
Из этого руководства вы узнали, как установить и настроить MongoDB на виртуальной машине Windows. Теперь вы можете получить доступ к MongoDB на виртуальной машине Windows, выполнив действия, описанные в дополнительных разделах [документации по MongoDB](https://docs.mongodb.com/manual/).

