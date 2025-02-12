---
title: Управление ресурсами Azure Data Lake Storage 1-го поколения в обозревателе службы хранилища Azure
description: Сведения о доступе к данным и ресурсам Azure Data Lake Storage 1-го поколения, а также управление ими в обозревателе службы хранилища Azure
Keywords: Store Озера данных Azure, обозревателя службы хранилища Azure
services: Data Lake Store
documentationcenter: ''
author: jejiang
manager: DJ
editor: Jenny Jiang
ms.assetid: ''
ms.service: data-lake-store
ms.custom: Azure Data Lake Store
ms.devlang: dotnet
ms.topic: article
ms.date: 02/05/2018
ms.author: jejiang
ms.openlocfilehash: 18ef1f182611a9c8fbf24cd08026633f5449bbe9
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "60613621"
---
# <a name="manage-azure-data-lake-storage-gen1-resources-by-using-storage-explorer"></a>Управление ресурсами Azure Data Lake Storage 1-го поколения с помощью обозревателя службы хранилища

[Azure Data Lake Storage 1-го поколения](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-overview) представляет собой службу для хранения больших объемов неструктурированных данных, например текстовых или двоичных. Вы можете получить доступ к данным из любого места через протоколы HTTP или HTTPS. Через Data Lake Storage 1-го поколения в обозревателе службы хранилища Azure вы можете получать доступ к данным и ресурсам Data Lake Storage 1-го поколения и другим сущностям Azure, например большим двоичным объектам и очередям, а также управлять ими. Теперь это средство можно использовать для управления всеми объектами Azure в одном месте.

Другое преимущество заключается в том, что вам не нужно разрешение в подписке для управления данными Data Lake Storage 1-го поколения. В обозревателе службы хранилища вы можете добавить путь Data Lake Storage 1-го поколения к узлу **Локальные и присоединенные**, пока действует соответствующее разрешение.

## <a name="prerequisites"></a>Технические условия
Чтобы выполнить действия, описанные в этой статье, необходимо следующее.

*   Подписка Azure. См. страницу [бесплатной пробной версии Azure](https://azure.microsoft.com/pricing/free-trial).
*   Учетная запись Data Lake Storage 1-го поколения. За инструкциями по созданию учетной записи обращайтесь к статье [Начало работы с Azure Data Lake Storage 1-го поколения](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-get-started-portal).

## <a name="install-storage-explorer"></a>Установка Обозревателя службы хранилища

Скачать новейшие версии Обозревателя службы хранилища Azure можно на странице [Обозреватель хранилища Azure](https://azure.microsoft.com/features/storage-explorer/). Теперь поддерживаются версии для Windows, Linux и MAC.

## <a name="connect-to-an-azure-subscription"></a>Подключение к подписке Azure

1. В Обозревателе службы хранилища Azure щелкните значок подключаемого модуля слева.
       
   ![Значок подключаемого модуля](./media/data-lake-store-in-storage-explorer/plug-in-icon.png)
 
2. Выберите **Add an Azure Account** (Добавить учетную запись Azure), а затем щелкните **Войти**.

   ![Диалоговое окно подключения к службе хранилища Azure](./media/data-lake-store-in-storage-explorer/connect-to-azure-subscription.png)

2. В диалоговом окне **Sign in to your account** (Вход в учетную запись) введите свои учетные данные Azure.

    ![Диалоговое окно входа в Azure](./media/data-lake-store-in-storage-explorer/sign-in.png)

3. Выберите свою подписку в списке и нажмите кнопку **Применить**.

    ![Сведения о подписке и кнопка "Применить"](./media/data-lake-store-in-storage-explorer/apply-subscription.png)

    На панели **обозревателя** будут показаны учетные записи в выбранной подписке.

    ![Список учетных записей](./media/data-lake-store-in-storage-explorer/account-list.png)

Вы успешно подключили Data Lake Storage 1-го поколения к подписке Azure.

## <a name="connect-to-data-lake-storage-gen1"></a>Подключение к Data Lake Storage 1-го поколения
Вы можете получить доступ к ресурсам, которых нет в вашей подписке, если вам предоставят универсальный код ресурса (URI) для ресурсов. Вы можете подключиться к Data Lake Storage 1-го поколения с его помощью после входа в систему.
1. Откройте обозреватель хранилищ.
2. В левой области разверните узел **Local and Attached** (Локальные и присоединенные).
3. Щелкните правой кнопкой мыши **Data Lake Store** и выберите команду **Connect to Data Lake Store** (Подключиться к Data Lake Store).

      ![Команда подключения к Data Lake Store в контекстном меню](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-uri-attach.png)

4. Введите URI. Средство перейдет в расположение введенного URL-адреса.

      ![Диалоговое окно подключения к Data Lake Store с текстовым полем для ввода URI](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-uri-attach-dialog.png)

      ![Результат подключения к Data Lake Storage 1-го поколения](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-attach-finish.png)

## <a name="view-a-data-lake-storage-gen1-accounts-contents"></a>Просмотр содержимого учетной записи Data Lake Storage 1-го поколения
В ресурсах учетной записи Data Lake Storage 1-го поколения содержатся файлы и папки.

В шагах ниже описывается, как просмотреть содержимое учетной записи Data Lake Storage 1-го поколения в обозревателе службы хранилища.

1. Откройте обозреватель хранилищ.
2. В левой области разверните подписку, в которой содержится нужная учетная запись Data Lake Storage 1-го поколения.
3. Разверните **Data Lake Store**.
4. Щелкните правой кнопкой мыши узел учетной записи Data Lake Storage 1-го поколения, который необходимо просмотреть, и выберите **Открыть**. Кроме того, чтобы открыть учетную запись Data Lake Storage 1-го поколения, можно дважды щелкнуть ее. 
   
   На главной панели отобразится содержимое учетной записи Data Lake Storage 1-го поколения.

   ![Главная панель со списком папок](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-toolbar-mainpane.png) 

## <a name="manage-resources-in-data-lake-storage-gen1"></a>Управление ресурсами в Data Lake Storage 1-го поколения

Вы можете управлять ресурсами Data Lake Storage 1-го поколения. Для этого доступны следующие операции:
*   Просмотр ресурсов Data Lake Storage 1-го поколения в нескольких учетных записях Data Lake Storage 1-го поколения.  
*   Использование строки подключения, чтобы напрямую подключиться к Data Lake Storage 1-го поколения и управлять им. 
*   Просмотр ресурсов Data Lake Storage 1-го поколения, к которым другие пользователи предоставили общий доступ, с помощью списка управления доступом (ACL) в узле **Локальные и присоединенные**.
*   Выполнение операций CRUD в папках и файлах: поддержка рекурсивных папок и множественного выбора файлов. 
*   Перетаскивание, удаление и добавление папки для быстрого доступа к последним расположениям. Эта операция соответствует возможностям настольной версии проводника. 
*   Копирование и открытие гиперссылки Data Lake Storage 1-го поколения с помощью обозревателя службы хранилища в один щелчок. 
*   Отображение журнала действий в нижней правой области для просмотра состояния действия.
*   Отображение статистики папки и свойств файла.

## <a name="manage-resources-in-azure-storage-explorer"></a>Управление ресурсами в обозревателе службы хранилища Azure
После создания учетной записи Data Lake Storage 1-го поколения вы сможете:

* передавать папки и файлы, а также загружать и открывать ресурсы на локальном компьютере;
* закреплять ресурсы для **быстрого доступа**, создавать папки, копировать URL-адреса и выбирать все ресурсы;
* копировать, вставлять, переименовывать, удалять, просматривать статистику папки и обновлять.

С помощью элементов ниже показано, как управлять ресурсами в учетной записи Data Lake Storage 1-го поколения. Следуйте приведенным ниже инструкциям, чтобы выполнить необходимую задачу.

### <a name="upload-files"></a>Upload files

1. На панели инструментов в главной области нажмите кнопку **Отправить**, а затем в раскрывающемся меню выберите **Отправить файлы**.

   ![Пункт меню "Отправить файлы"](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-upload-files-menu.png) 

2. В диалоговом окне **Select files to Upload** (Выбор файлов для отправки) выберите нужные файлы.

   ![Диалоговое окно для отправки файлов](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-upload-files-dialog.png)

3. Выберите **Открыть**, чтобы начать отправку.

### <a name="upload-a-folder"></a>Отправка папки

1. На панели инструментов в основной области нажмите кнопку **Отправить**, а затем в раскрывающемся меню выберите **Upload Folder** (Отправить папку).

   ![Пункт меню Upload Folder (Отправить папку)](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-upload-folder-menu.png) 
     
2. В диалоговом окне **Select folder to upload** (Выбор папки для отправки) выберите нужную папку. Затем щелкните **Выбрать папку**.

   ![Диалоговое окно для отправки папок](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-upload-folder-dialog.png)      

   Начнется загрузка.

   ![Диалоговое окно выполнения отправки](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-upload-folder-drag.png) 

> [!NOTE]
> Вы можете напрямую перетащить папки и файлы на локальный компьютер, чтобы начать отправку. 
       
### <a name="download-folders-or-files-to-your-local-computer"></a>Загрузка папок или файлов на локальный компьютер

1. Выберите папки или файлы, которые вы хотите загрузить.
2. На панели инструментов в основной области нажмите кнопку **Скачать**.
3. В диалоговом окне **Select a folder to save the downloaded files into** (Выбор папки, куда нужно сохранить загруженные файлы) укажите расположение и имя.
4. Щелкните **Сохранить**.

### <a name="open-a-folder-or-file-from-your-local-computer"></a>Открытие папки или файла с локального компьютера

1. Выберите папку или файл, которые необходимо открыть.
2. На панели инструментов в основной области нажмите кнопку **Открыть**. Кроме того, вы можете щелкнуть выбранную папку или файл правой кнопкой мыши и в контекстном меню выбрать **Открыть**.

Файл будет скачан и открыт с помощью приложения, выбор которого зависит от базового типа файла. Папка же откроется в основной области.

![Открытый файл](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-open.png) 

### <a name="copy-folders-or-files-to-the-clipboard"></a>Копирование файлов и папок в буфер обмена

1. Выберите папки или файлы, которые необходимо скопировать.
2. На панели инструментов в основной области нажмите кнопку **Копировать**. Кроме того, вы можете щелкнуть выбранные папки или файлы правой кнопкой мыши и в контекстном меню выбрать **Копировать**.
3. В левой области перейдите в другую учетную запись Data Lake Storage 1-го поколения и дважды щелкните ее, чтобы она отобразилась в основной области.
4. На панели инструментов в основной области выберите **Вставить**, чтобы создать копию. Кроме того, вы можете щелкнуть в контекстном меню команду **Вставить** в месте назначения.

![Выбранные элементы для копирования папки](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-copy-paste.png)

> [!NOTE]
> Операции копирования и вставки между разными типами хранилища не поддерживаются. Вы можете скопировать папки или файлы Data Lake Storage 1-го поколения и вставить их в другую учетную запись Data Lake Storage 1-го поколения. Но *вы не можете* копировать файлы или папки Data Lake Storage 1-го поколения и вставлять их в хранилище BLOB-объектов Azure или наоборот.
> 
> Для выполнения операции копирования и вставки нужно загрузить папки или файлы на локальный компьютер, а потом передать их оттуда в расположения назначения. В серверной части эти операции *не выполняются*. Операция копирования и вставки больших файлов может занять много времени. Метод оптимизации копирования и перемещения файлов находится в разработке.

### <a name="delete-folders-or-files"></a>Удаление файлов или папок

1. Выберите папки или файлы, которые нужно удалить.
2. На панели инструментов в основной области нажмите кнопку **Удалить**. Кроме того, вы можете щелкнуть выбранную папку или файл правой кнопкой мыши и в контекстном меню выбрать **Удалить**.
3. Щелкните **Да** в диалоговом окне подтверждения.

![Выбранные элементы для удаления папки](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-delete.png)

### <a name="pin-to-quick-access"></a>Закрепление для быстрого доступа

1. Выберите папку, которую необходимо закрепить.
2. На панели инструментов в основной области нажмите кнопку **Закрепить для быстрого доступа**.

   В левой области вы увидите, что выбранная папка добавлена в узел **Быстрый доступ**.

   ![Выбранные элементы для закрепления папки для быстрого доступа](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-quick-access.png)

После закрепления папки для узла **быстрого доступа**, можно легко получить доступ к ресурсам.

### <a name="use-deep-links"></a>Использование прямых ссылок
Если доступен URL-адрес, его можно просто ввести в путь адреса в проводнике или браузере. Затем для перехода в расположение URL-адреса автоматически запустится файл Storage Explorer.exe.

![Прямая ссылка в проводнике](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-deep-link.png)


## <a name="next-steps"></a>Дальнейшие действия
* Последние заметки о выпуске Обозревателя службы хранилищ и связанные с ним видео см. [здесь](https://www.storageexplorer.com).
* Сведения об управлении Azure DB Cosmos в Обозревателе службы хранилища Azure см. в [этой статье](https://docs.microsoft.com/azure/cosmos-db/storage-explorer).
* [Приступая к работе с обозревателем службы хранилища (предварительная версия)](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer).
* [Начало работы с Azure Data Lake Storage 1-го поколения](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-overview).
* Просмотрите видео на сайте [YouTube](https://www.youtube.com/watch?v=iNIbg1DLgWo&feature=youtu.be), чтобы узнать, как использовать Azure Cosmos DB в Обозревателе службы хранилища Azure.
