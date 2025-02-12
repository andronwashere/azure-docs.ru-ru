---
title: Как использовать Hudson с хранилищем BLOB-объектов | Документация Майкрософт
description: В данном уроке подробно описывается использование Hudson с хранилищем BLOB-объектов Azure в качестве хранилища артефактов построения.
services: storage
author: seguler
ms.service: storage
ms.devlang: Java
ms.topic: article
ms.date: 02/28/2017
ms.author: tarcher
ms.subservice: common
ms.openlocfilehash: 54e91d4df1109b9ece1150f8b44665789e4dfce1
ms.sourcegitcommit: de47a27defce58b10ef998e8991a2294175d2098
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/15/2019
ms.locfileid: "67875891"
---
# <a name="using-azure-storage-with-a-hudson-continuous-integration-solution"></a>Использование хранилища Azure с решением непрерывной интеграции Hudson
## <a name="overview"></a>Краткое описание
Приведенные ниже сведения демонстрируют использование хранилища BLOB-объектов в качестве репозитория для артефактов построения, созданных решением непрерывной интеграции Hudson, или в качестве источника загружаемых файлов для процесса построения. Одним из сценариев, где это может оказаться полезным, является программирование в гибкой среде разработки (с помощью Java или других языков), построения выполняются на основе непрерывной интеграции, и вам требуется репозиторий для артефактов построения, чтобы можно было, например, использовать их совместно с другими членами организации, вашими клиентами или вести архив.  Еще один сценарий связан с ситуацией, когда задание построения само требует других файлов, например, зависимостей для загрузки в качестве входа построения.

В этом учебнике будет использоваться подключаемый модуль службы хранилища Azure для решения непрерывной интеграции Hudson, предоставляемый корпорацией Майкрософт.

## <a name="introduction-to-hudson"></a>Общая информация о Hudson
Hudson обеспечивает непрерывную интеграцию программного проекта, позволяя разработчикам легко интегрировать свои изменения кода и автоматизировать процесс построения сборок, что повышает производительность их труда. Для построений осуществляется контроль версий, а артефакты построения можно загрузить в разные репозитории. В этом разделе демонстрируется использование хранилища больших двоичных объектов Azure в качестве репозитория артефактов построения. Здесь также рассматривается загрузка зависимостей из хранилища больших двоичных объектов Azure.

Дополнительные сведения о Hudson см. в разделе [Знакомство с Hudson](https://wiki.eclipse.org/Hudson-ci/Meet_Hudson).

## <a name="benefits-of-using-the-blob-service"></a>Преимущества использования службы BLOB-объектов
Преимущества использования службы BLOB-объектов для размещения артефактов построения при гибкой разработке включают в себя следующее:

* Высокая доступность артефактов построения и (или) загружаемых зависимостей.
* Производительность при отправке артефактов построения решением Hudson CI.
* Производительность при загрузке артефактов построения клиентами и партнерами.
* Контроль политики доступа пользователей с возможностью выбора между анонимным доступом, общим доступом на основе срока действия, доступом по подписи, закрытым доступом и т. п.

## <a name="prerequisites"></a>Предварительные требования
Чтобы использовать службу BLOB-объектов с решением Hudson, необходимо следующее:

* Решение непрерывной интеграции Hudson.
  
    Если у вас нет решения Hudson, его можно запустить следующим образом:
  
  1. На компьютере с поддержкой Java скачайте WAR-файл Hudson с сайта <http://hudson-ci.org/>.
  2. В командной строке, открытой на папке с WAR-файлом Hudson, запустите этот файл. Например, если вы загрузили версию 3.1.2:
     
      `java -jar hudson-3.1.2.war`

  3. Откройте в браузере `http://localhost:8080/`. При этом открывается панель мониторинга Hudson.
  4. При первом использовании Hudson выполните начальную настройку по адресу `http://localhost:8080/`.
  5. После завершения начальной настройки отмените запущенный экземпляр Hudson WAR, снова запустите WAR-файл Hudson и заново откройте панель мониторинга Hudson по адресу `http://localhost:8080/`, которая будет использоваться для установки и настройки подключаемого модуля хранилища Azure.
     
      Хотя обычно решение Hudson настраивается на запуск в качестве службы, в рамках данного учебника будет достаточно запуска WAR-файла Hudson в командной строке.
* Учетная запись Azure. Вы можете зарегистрировать учетную запись Azure по адресу <https://www.azure.com>.
* Учетная запись хранения Azure. Если у вас еще нет учетной записи хранения, ее можно создать с помощью процедуры [Создание учетной записи хранения](../common/storage-quickstart-create-account.md).
* Знакомство с решением Hudson CI рекомендуется, но не является обязательным, поскольку ниже будет приведен простой пример, показывающий этапы использования службы BLOB-объектов в качестве репозитория для артефактов построения решения Hudson CI.

## <a name="how-to-use-the-blob-service-with-hudson-ci"></a>Использование службы BLOB-объектов с решением непрерывной интеграции Hudson
Чтобы использовать службу BLOB-объектов с решением Hudson, необходимо установить подключаемый модуль хранилища Azure, настроить этот модуль на использование учетной записи хранения, а затем создать действие, выполняемое после построения и отправляющее артефакты построения в вашу учетную запись хранения. Эти действия описаны в следующих разделах.

## <a name="how-to-install-the-azure-storage-plugin"></a>Как установить подключаемый модуль службы хранилища Azure
1. На панели мониторинга Hudson щелкните элемент **Manage Hudson**(Управление Hudson).
2. На странице **Управление Hudson** щелкните **Управление подключаемыми модулями**.
3. Перейдите на вкладку **Available** (Доступный).
4. Нажмите кнопку **Others**(Другие).
5. В разделе **Средства отправки артефактов** установите флажок **Подключаемый модуль службы хранилища Microsoft Azure**.
6. Щелкните **Install**(Установить).
7. После завершения установки перезапустите Hudson.

## <a name="how-to-configure-the-azure-storage-plugin-to-use-your-storage-account"></a>Как настроить подключаемый модуль службы хранилища Azure для использования своей учетной записи хранения
1. На панели мониторинга Hudson щелкните элемент **Manage Hudson**(Управление Hudson).
2. На странице **Управление Hudson** щелкните **Настройка системы**.
3. В разделе **Microsoft Azure Storage Account Configuration** (Настройка учетной записи хранения Azure):
   
    а) Введите имя своей учетной записи хранения, которое можно получить на [портале Azure](https://portal.azure.com).
   
    б) Введите ключ учетной записи хранения, который также можно получить через [портал Azure](https://portal.azure.com).
   
    в) Используйте значение по умолчанию для параметра **Blob Service Endpoint URL** (URL-адрес конечной точки службы BLOB-объектов) при использовании общедоступного облака Azure. При использовании другого облака Azure используйте конечную точку, указанную для вашей учетной записи хранения на [портале Azure](https://portal.azure.com).
   
    г) Щелкните **Validate storage credentials** (Проверить учетные данные хранилища) для проверки вашей учетной записи хранения.
   
    д) [Необязательно] Если у вас есть дополнительные учетные записи хранения, которые требуется сделать доступными для решения Hudson CI, нажмите **Добавить дополнительные учетные записи хранения**.
   
    е) Нажмите кнопку **Save** (Сохранить), чтобы сохранить настройки.

## <a name="how-to-create-a-post-build-action-that-uploads-your-build-artifacts-to-your-storage-account"></a>Создание действия, выполняемого после построения и отправляющего артефакты построения в вашу учетную запись хранения
В учебных целях сначала необходимо создать задание, которое создаст несколько файлов, а затем добавить выполняемое после построения действие для отправки этих файлов в вашу учетную запись хранения.

1. На панели мониторинга Hudson щелкните элемент **New Job**(Создать задание).
2. Назовите задание **MyJob**, щелкните **Построение программного задания в свободном стиле** и нажмите кнопку **OK**.
3. В разделе **Построение** конфигурации задания щелкните **Добавить шаг построения** и выберите пункт **Выполнить пакетную команду Windows**.
4. В поле **Command**(Команда) используйте следующие команды:

    ```   
        md text
        cd text
        echo Hello Azure Storage from Hudson > hello.txt
        date /t > date.txt
        time /t >> date.txt
    ```

5. В разделе конфигурации задания **Действия после построения** щелкните **Передать артефакты в хранилище BLOB-объектов Azure**.
6. В поле **Storage Account Name**(Имя учетной записи хранения) выберите используемую учетную запись хранения.
7. Для поля **Container name** задайте имя контейнера. (Если этот контейнер не существует, он будет создан при передаче артефактов построения.) Вы можете использовать переменные среды, поэтому для данного примера введите имя контейнера **${JOB_NAME}** .
   
    **Подсказка**
   
    В разделе **Команда**, где вы указали сценарий в поле **Выполнить пакетную команду Windows**, находится ссылка на переменные среды, которые распознаются в решении Hudson. Выберите эту ссылку, чтобы узнать имена и описания переменных среды. Обратите внимание, что переменные среды, которые содержат специальные символы, такие как переменная среды **BUILD_URL**, нельзя использовать в качестве имени контейнера или общего виртуального пути.
8. Щелкните **Сделать новый контейнер открытым по умолчанию** для этого примера. (Если вы хотите использовать закрытый контейнер, необходимо создать подпись общего доступа. Это выходит за рамки данной статьи. Дополнительные сведения о подписанных URL-адресах см. в статье [Использование подписанных URL-адресов (SAS)](../storage-dotnet-shared-access-signature-part-1.md)).
9. [Необязательно] Щелкните **Clean container before uploading** (Очистить контейнер перед отправкой), если хотите, чтобы контейнер был перед отправкой артефактов построения (не устанавливайте этот флажок, если не хотите очищать содержимое контейнера).
10. В поле **List of Artifacts to upload** (Список передаваемых артефактов) введите значение **text/*.txt**.
11. Для параметра **Общий виртуальный путь для отправленных артефактов** введите значение **${BUILD\_ID}/${BUILD\_NUMBER}** .
12. Нажмите кнопку **Save** (Сохранить), чтобы сохранить настройки.
13. На панели мониторинга Hudson щелкните **Построение**, чтобы запустить **MyJob**. Изучите выходные данные в консоли для определения состояния. Сообщения о состоянии для хранилища Azure будут включены в выходные данные консоли, когда действие после построения начнет передавать артефакты построения.
14. После успешного завершения задания можно просмотреть артефакты построения, открыв общедоступный BLOB-объект.
    
    а) Войдите на [портал Azure](https://portal.azure.com).
    
    б) Щелкните **Хранилище**.
    
    в) Щелкните имя учетной записи хранения, используемой для решения Hudson.
    
    г) Щелкните **Контейнеры**.
    
    д) Выберите контейнер с именем **myjob**, которое является копией имени задания Hudson, написанного в нижнем регистре. Имена контейнеров и больших двоичных объектов в хранилище Azure отображаются строчными буквами (и с учетом регистра). В списке BLOB-объектов для контейнера с именем **myjob** вы увидите **hello.txt** и **date.txt**. Скопируйте URL-адрес любого из этих элементов и откройте его в браузере. Вы увидите текстовый файл, который был загружен в качестве артефакта построения.

На каждое задание может быть создано только одно действие после построения, которое отправляет артефакты в хранилище больших двоичных объектов Azure. Обратите внимание, что одно действие после построения по загрузке артефактов в хранилище больших двоичных объектов Azure может определять различные файлы (в том числе подстановочные знаки) и пути для файлов в **Списке артефактов для отправки** с использованием точки с запятой в качестве разделителя. Например, если операция сборки Hudson создает JAR-файлы и TXT-файлы в папке **build** рабочей области, при этом их нужно отправить в хранилище BLOB-объектов Azure, укажите в поле **Список отправляемых артефактов** следующее значение: **build/\*.jar;build/\*.txt**. Чтобы задать путь для использования в имени blob-объекта, можно также использовать синтаксис с двойным двоеточием. Например, если необходимо отправить JAR-файлы с использованием **двоичных файлов** в пути BLOB-объекта, а TXT-файлы — с использованием **уведомлений** в пути BLOB-объекта, используйте для параметра **Список отправляемых артефактов** следующее значение: **build/\*.jar::binaries;build/\*.txt::notices**.

## <a name="how-to-create-a-build-step-that-downloads-from-azure-blob-storage"></a>Создание шага сборки для скачивания больших двоичных объектов из службы хранилища Azure
В последующих шагах показана настройка шага построения для загрузки больших двоичных объектов из хранилища Azure. Это может оказаться полезным при необходимости включить элементы в построение, например JAR-файлы, которые хранятся в хранилище больших двоичных объектов Azure.

1. В разделе **Построение** конфигурации задания щелкните **Добавить шаг построения** и выберите пункт **Загрузить из хранилища BLOB-объектов Azure**.
2. В поле **Storage Account Name**(Имя учетной записи хранения) выберите используемую учетную запись хранения.
3. Для поля **Container name**(Имя контейнера) задайте имя контейнера с blob-объектами, которое нужно загрузить. Можно использовать переменные среды.
4. Для поля **Blob name**задайте имя blob-объекта. Можно использовать переменные среды. В то же время можно использовать звездочку как подстановочный символ после задания начальной буквы (букв) имени blob-объекта. Например, **Project\\** * будет указывать все большие двоичные объекты, имена которых начинаются с **Project**.
5. [Необязательно] В поле **Путь загрузки**укажите, в какую папку на компьютере Hudson нужно загрузить файлы из хранилища больших двоичных объектов Azure. Переменные среды также могут использоваться. (Если не задать значения для поля **Download path**, файлы из хранилища больших двоичных объектов Azure будут загружаться в рабочую область задания.)

При наличии дополнительных элементов, которые нужно загрузить из хранилища больших двоичных объектов Azure, можно создать дополнительные шаги построения.

После выполнения построения можно проверить выход консоли истории построения или взглянуть на местоположение для загрузки и удостовериться, успешно ли загрузились необходимы blob-объекты.

## <a name="components-used-by-the-blob-service"></a>Компоненты, используемые службой BLOB-объектов
Ниже приведен обзор компонентов службы BLOB-объектов:

* **Учетная запись хранения**. Весь доступ к хранилищу Azure осуществляется с помощью учетной записи хранения. Это самое высокоуровневое пространство имен для доступа к BLOB-объектам. Учетная запись может содержать неограниченное число контейнеров при условии, что их общий размер не достигает 100 ТБ.
* **Контейнер.** Контейнер обеспечивает группирование набора больших двоичных объектов. Все BLOB-объекты должны содержаться в контейнере. Учетная запись может содержать неограниченное число контейнеров. Контейнер может хранить неограниченное количество больших двоичных объектов.
* **Большой двоичный объект.** файл любого типа и размера. Существует два типа BLOB-объектов, которые могут храниться в хранилище Azure: блочные и страничные. Большинство файлов являются блочными BLOB-объектами. Единичный блочный BLOB-объект может иметь размер до 200 ГБ. В этом учебнике используются блочные BLOB-объекты. Страничные BLOB-объекты, другой BLOB-объект могут иметь размер до 1 ТБ. Они более эффективны, когда диапазоны байтов в файле часто изменяются. Дополнительные сведения о BLOB-объектах см. в разделе [Основные сведения о блочных, добавочных и страничных BLOB-объектах](https://msdn.microsoft.com/library/azure/ee691964.aspx).
* **Формат URL-адреса**. К большому двоичному объекту можно обратиться, используя следующий формат URL-адреса:
  
    `http://storageaccount.blob.core.windows.net/container_name/blob_name`
  
    (Приведенный выше формат применяется для общедоступного облака Azure. При использовании другого облака Azure используйте конечную точку на [портале Azure](https://portal.azure.com), чтобы определить конечную точку URL-адреса.)
  
    В указанном выше формате `storageaccount` представляет имя вашей учетной записи хранения, `container_name` представляет имя контейнера, а `blob_name` представляет имя BLOB-объекта соответственно. В имени контейнера может использовать несколько путей, разделенных косой чертой — **/** . Примером имени контейнера в данном учебнике было имя **MyJob**, а имя **${BUILD\_ID}/${BUILD\_NUMBER}** использовалось в качестве общего виртуального пути, в результате чего BLOB-объект имеет следующий URL-адрес:
  
    `http://example.blob.core.windows.net/myjob/2014-05-01_11-56-22/1/hello.txt`

## <a name="next-steps"></a>Дальнейшие действия
* [Знакомство с Hudson](https://wiki.eclipse.org/Hudson-ci/Meet_Hudson)
* [Пакет SDK для службы хранилища Azure для Java](https://github.com/azure/azure-storage-java)
* [справочнике по пакету SDK для клиента службы хранилища Azure](http://dl.windowsazure.com/storage/javadoc/)
* [API-интерфейс REST служб хранилища Azure](https://msdn.microsoft.com/library/azure/dd179355.aspx)
* [Блог рабочей группы службы хранилища Azure](https://blogs.msdn.com/b/windowsazurestorage/)

Дополнительные сведения см. в разделе [Azure for Java developers](/java/azure) (Azure для разработчиков Java).
