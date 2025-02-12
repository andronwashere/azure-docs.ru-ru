---
title: Восстановление данных из Azure на сервер Windows Server или компьютер Windows
description: Узнайте, как восстановить данные из Azure на сервер Windows Server или компьютер Windows.
author: saurabhsensharma
manager: shivamg
ms.service: backup
ms.topic: conceptual
ms.date: 9/7/2018
ms.author: saurse
ms.openlocfilehash: e22db587c698594170c3b08ea04ff6699b6a61c8
ms.sourcegitcommit: c72ddb56b5657b2adeb3c4608c3d4c56e3421f2c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/24/2019
ms.locfileid: "68465677"
---
# <a name="restore-files-to-windows-by-using-the-azure-resource-manager-deployment-model"></a>Восстановление файлов в Windows с помощью модели развертывания Azure Resource Manager

В этой статье описывается процесс восстановления данных из хранилища службы архивации. Чтобы восстановить данные, используйте мастер восстановления данных, доступный в агенте служб восстановления Microsoft Azure (MARS). Можно выполнить следующие действия:

* Восстановление данных на тот же компьютер, с которого создавались резервные копии.
* Восстановление файлов на другой компьютер.

Используйте возможность мгновенного восстановления, чтобы подключить моментальный снимок точки восстановления, доступный для записи, в качестве тома восстановления. Вы сможете изучить этот том восстановления и выборочно восстановить файлы из него, то есть скопировать их на локальный компьютер.

> [!NOTE]
> Если вы хотите использовать мгновенное восстановление данных, необходимо использовать [обновление службы архивации Azure за январь 2017 г.](https://support.microsoft.com/en-us/help/3216528?preview) Кроме того, архивные данные должны быть защищены в хранилищах в местонахождениях, перечисленных в статье технической поддержки. В описании [обновления службы архивации Azure за январь 2017 г.](https://support.microsoft.com/en-us/help/3216528?preview) вы найдете актуальный список регионов, поддерживающих мгновенное восстановление.
>

Используйте мгновенное восстановление с помощью хранилищ служб восстановления на портале Azure. Если вы хранили данные в хранилищах службы архивации, они были преобразованы в хранилища служб восстановления. Если вы хотите использовать мгновенное восстановление, загрузите обновление служб восстановления Microsoft Azure и выполните процедуры, в которых упоминается мгновенное восстановление.

[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]

## <a name="use-instant-restore-to-recover-data-to-the-same-machine"></a>Восстановление данных на тот же компьютер с помощью мгновенного восстановления

Если вы случайно удалили файл и хотите восстановить его на том же компьютере (с которого создана резервная копия), указанные ниже действия помогут вам восстановить данные.

1. Откройте оснастку **Microsoft Azure Backup**. Если вы не знаете, куда была установлена оснастка, найдите на компьютере или сервере службу **Microsoft Azure Backup**.

    Классическое приложение должны быть в результатах поиска.

2. Выберите **Восстановить данные**, чтобы запустить мастер.

    ![Снимок экрана Azure Backup с выделенным элементом "Восстановить данные"](./media/backup-azure-restore-windows-server/recover.png)

3. Чтобы восстановить данные на тот же сервер или компьютер, в области **Приступая к работе** выберите **Этот сервер (`<server name>`)**  > **Далее**.

    ![Снимок экрана страницы "Приступая к работе" мастера восстановления данных](./media/backup-azure-restore-windows-server/samemachine_gettingstarted_instantrestore.png)

4. На странице **Выбор режима восстановления** последовательно выберите **отдельные файлы и папки** > **Далее**.

    ![Снимок экрана страницы "Выбор режима восстановления" мастера восстановления данных](./media/backup-azure-restore-windows-server/samemachine_selectrecoverymode_instantrestore.png)
   > [!IMPORTANT]
   > Для параметра восстановления отдельных файлов и папок требуется .NET Framework 4.5.2 или более поздней версии. Если параметр **Отдельные файлы и папки** не отображается, обновите .NET Framework до версии 4.5.2 или выше и повторите попытку.

   > [!TIP]
   > Параметр **Отдельные файлы и папки** позволяет быстро получить доступ к данным точек восстановления. Параметр подходит для восстановления отдельных файлов с общим размером не более 80 ГБ, а также обеспечивает скорость передачи или копирования до 6 Мбит/с во время восстановления. Параметр **Том** позволяет восстановить все резервные копии на определенном томе. Этот параметр обеспечивает более высокую скорость передачи (до 60 Мбит/с), что идеально подходит для восстановления большого объема данных или целых томов.

5. На странице **Выбор тома и даты** выберите том, содержащий файлы и папки, который необходимо восстановить.

    В календаре выберите точку восстановления. Даты **полужирным** шрифтом означают доступность по крайней мере одной точки восстановления. Если в пределах одной даты доступно несколько точек восстановления, выберите конкретную точку восстановления из раскрывающегося меню **Время**.

    ![Снимок экрана страницы "Том и дата" мастера восстановления данных](./media/backup-azure-restore-windows-server/samemachine_selectvolumedate_instantrestore.png)

6. Выбрав точку восстановления, щелкните **Подключить**.

    Служба архивации Azure подключит локальную точку восстановления и использует ее в качестве тома для восстановления.

7. На странице **Поиск и восстановление файлов** выберите **Обзор**, чтобы открыть проводник Windows и найти нужные файлы и папки.

    ![Снимок экрана страницы "Поиск и восстановление файлов" мастера восстановления данных](./media/backup-azure-restore-windows-server/samemachine_browserecover_instantrestore.png)


8. В проводнике Windows скопируйте файлы и папки, которые требуется восстановить, и вставьте их в любую локальную папку на сервере или компьютере. Вы можете открыть или передать потоком эти файлы непосредственно из тома восстановления и затем убедиться, что восстанавливаются правильные версии.

    ![Снимок экрана проводника Windows с выделенным пунктом "Копировать"](./media/backup-azure-restore-windows-server/samemachine_copy_instantrestore.png)


9. По завершении на странице **Поиск и восстановление файлов** нажмите кнопку **Отключить**. Нажмите кнопку **Да**, чтобы подтвердить отключение тома.

    ![Снимок экрана страницы "Поиск и восстановление файлов" мастера восстановления данных](./media/backup-azure-restore-windows-server/samemachine_unmount_instantrestore.png)

    > [!Important]
    > Если не нажать кнопку **Отключить**, то том восстановления останется подключенным в течение 6 часов с момента подключения. В случае, если выполняется копирование файлов, время подключения продлевается максимум на 24 часа. И пока он останется подключенным, невозможно будет выполнять какие-либо операции архивации. Все операции резервного копирования, запланированные на период, когда том восстановления подключен, будут выполнены после его отключения.
    >


## <a name="use-instant-restore-to-restore-data-to-an-alternate-machine"></a>Использование мгновенного восстановления для восстановления данных на другой компьютер
При потере всего содержимого сервера вы все равно можете восстановить данные из службы архивации Azure на другом компьютере. Соответствующий рабочий процесс описан ниже.


В нем используются следующие термины:

* *Исходный компьютер* — компьютер для данных, на котором была создана резервная копия и который в настоящий момент недоступен.
* *Целевой компьютер* — компьютер, на который восстанавливаются данные.
* *Пример хранилища* — хранилище служб восстановления, в котором зарегистрированы исходный и целевой компьютеры. <br/>

> [!NOTE]
> Резервные копии невозможно восстановить на целевой компьютер с более ранней версией операционной системы. Например, резервную копию, созданную на компьютере с ОС Windows 7, можно восстановить на компьютере под Windows 7 (или более поздней версии). а резервную копию, созданную на компьютере Windows 8, невозможно восстановить на компьютере Windows 7.
>
>

1. Откройте на целевом компьютере оснастку **Microsoft Azure Backup**.

2. Зарегистрируйте целевой и исходный компьютеры в одном хранилище Служб восстановления.

3. Выберите **Восстановить данные**, чтобы открыть **мастер восстановления данных**.

    ![Снимок экрана Azure Backup с выделенным элементом "Восстановить данные"](./media/backup-azure-restore-windows-server/recover.png)

4. На странице **Приступая к работе** выберите **Другой сервер**.

    ![Снимок экрана страницы "Приступая к работе" мастера восстановления данных](./media/backup-azure-restore-windows-server/alternatemachine_gettingstarted_instantrestore.png)

5. Укажите файл с учетными данными хранилища, который соответствует примеру хранилища, и выберите **Далее**.

    Если файл с учетными данными хранилища недействителен (или просрочен), скачайте новый файл из примера хранилища на портале Azure. После ввода действительных учетных данных хранилища появится имя соответствующего хранилища архивации.


6. На странице **Выбор резервного сервера** из списка компьютеров выберите исходный компьютер и укажите парольную фразу. Затем нажмите кнопку **Далее**.

    ![Снимок экрана страницы "Выбор резервного сервера" мастера восстановления данных](./media/backup-azure-restore-windows-server/alternatemachine_selectmachine_instantrestore.png)

7. В области **Выбор режима восстановления** выберите **Отдельные файлы и папки** > **Далее**.

    ![Снимок экрана страницы "Выбор режима восстановления" мастера восстановления данных](./media/backup-azure-restore-windows-server/alternatemachine_selectrecoverymode_instantrestore.png)

8. На странице **Выбор тома и даты** выберите том, содержащий файлы и папки, который необходимо восстановить.

    В календаре выберите точку восстановления. Даты **полужирным** шрифтом означают доступность по крайней мере одной точки восстановления. Если в пределах одной даты доступно несколько точек восстановления, выберите конкретную точку восстановления из раскрывающегося меню **Время**.

    ![Снимок экрана страницы "Том и дата" мастера восстановления данных](./media/backup-azure-restore-windows-server/alternatemachine_selectvolumedate_instantrestore.png)

9. Нажмите кнопку **Подключить**, чтобы подключить точку восстановления в качестве локального тома восстановления на целевом компьютере.

10. На странице **Поиск и восстановление файлов** выберите **Обзор**, чтобы открыть проводник Windows и найти нужные файлы и папки.

    ![Снимок экрана страницы "Поиск и восстановление файлов" мастера восстановления данных](./media/backup-azure-restore-windows-server/alternatemachine_browserecover_instantrestore.png)

11. В проводнике Windows скопируйте файлы и папки из восстановленного тома и вставьте их в расположение на целевом компьютере. Вы можете открыть или передать потоком эти файлы непосредственно из тома восстановления, а затем убедиться, что восстанавливаются правильные версии.

    ![Снимок экрана проводника Windows с выделенным пунктом "Копировать"](./media/backup-azure-restore-windows-server/alternatemachine_copy_instantrestore.png)

12. По завершении на странице **Поиск и восстановление файлов** нажмите кнопку **Отключить**. Нажмите кнопку **Да**, чтобы подтвердить отключение тома.

    ![Снимок экрана страницы "Поиск и восстановление файлов" мастера восстановления данных](./media/backup-azure-restore-windows-server/alternatemachine_unmount_instantrestore.png)

    > [!Important]
    > Если не нажать кнопку **Отключить**, то том восстановления останется подключенным в течение 6 часов с момента подключения. В случае, если выполняется копирование файлов, время подключения продлевается максимум на 24 часа. И пока он останется подключенным, невозможно будет выполнять какие-либо операции архивации. Все операции резервного копирования, запланированные на период, когда том восстановления подключен, будут выполнены после его отключения.
    >

## <a name="next-steps"></a>Следующие шаги
Теперь после восстановления файлов и папок можно [управлять резервными копиями](backup-azure-manage-windows-server.md).
