---
title: Руководство по копированию данных на Диск Azure Data Box | Документация Майкрософт
description: Используйте это руководство, чтобы узнать, как скопировать данные на диск Azure Data Box
services: databox
author: alkohli
ms.service: databox
ms.subservice: disk
ms.topic: tutorial
ms.date: 07/23/2019
ms.author: alkohli
Customer intent: As an IT admin, I need to be able to order Data Box Disk to upload on-premises data from my server onto Azure.
ms.openlocfilehash: 336cc7dae00d06e38e4be8671f1cb11ed73e5edc
ms.sourcegitcommit: c556477e031f8f82022a8638ca2aec32e79f6fd9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/23/2019
ms.locfileid: "68414638"
---
::: zone target="docs"

# <a name="tutorial-copy-data-to-azure-data-box-disk-and-verify"></a>Руководство по Копирование данных на Диск Microsoft Azure Data Box и проверка

Это руководство содержит инструкции как копировать данные с главного компьютера, а затем создавать контрольные суммы для проверки целостности данных.

Из этого руководства вы узнаете, как выполнять следующие задачи:

> [!div class="checklist"]
> * Копирование данных на диск Data Box
> * Проверка данных

## <a name="prerequisites"></a>Предварительные требования

Перед тем как начать, убедитесь в следующем.
- Вы завершили [руководство Распаковка, подключение и разблокировка Диска Azure Data Box](data-box-disk-deploy-set-up.md).
- Диски разблокированы и подключены к клиентскому компьютеру.
- На клиентском компьютере, с которого вы будете копировать данные на диски, установлена [поддерживаемая операционная система](data-box-disk-system-requirements.md##supported-operating-systems-for-clients).
- Убедитесь, что предполагаемый тип хранилища для ваших данных соответствует [поддерживаемым типам хранилища](data-box-disk-system-requirements.md#supported-storage-types-for-upload).
- Просмотрите [ограничения размеров управляемых дисков в ограничениях размера для объектов Azure](data-box-disk-limits.md#azure-object-size-limits).


## <a name="copy-data-to-disks"></a>Копирование данных на диски

Перед копированием данных на диски, ознакомьтесь со следующими рекомендациями.

- Вы несете ответственность за копирование данных в папки, соответствующие необходимому формату данных. Например, скопируйте данные блочного блока в соответствующую ему папку. Если формат данных не соответствует соответствующей папке (типу хранилища), то на более позднем этапе загрузка данных в Azure завершится с ошибкой.
- При копировании данных убедитесь, что размер данных соответствует ограничениям размера, указанным в разделе [Azure Data Box Disk limits (Preview)](data-box-disk-limits.md) (Ограничения для дисков Azure Data Box (предварительная версия)).
- Если данные, которые загружаются с помощью диска Data Box, одновременно загружаются другими приложениями за пределами диска Data Box, это может привести к сбоям задания загрузки и повреждению данных.

   > [!IMPORTANT]
   >  Если при создании заказа вы указали управляемые диски в качестве одного из назначений для хранения данных, то действуют условия, приведенные в следующем разделе.

- Вы можете иметь только один управляемый диск с заданным именем в группе ресурсов во всех предварительно созданных папках и по всему Диску Data Box. Это значит, что виртуальные жесткие диски, отправленные в предварительно созданные папки, должны иметь уникальные имена. Убедитесь, что указанное имя не совпадает с уже существующим управляемым диском в группе ресурсов. Если виртуальные жесткие диски имеют одинаковые имена, только один виртуальный жесткий диск преобразуется в управляемый диск с таким именем. Другие виртуальные жесткие диски передаются в виде страничных BLOB-объектов в промежуточную учетную запись хранения.
- Всегда копируйте виртуальные жесткие диски в одну из предварительно созданных папок. Если вы копируете виртуальные жесткие диски за пределами этих папок или в созданную вами папку, они отправляются в учетную запись хранения Azure в виде страничных BLOB-объектов, а не управляемых дисков.
- Чтобы создать управляемые диски, можно отправить только фиксированные виртуальные жесткие диски. Динамические виртуальные жесткие диски, разностные виртуальные жесткие диски или VHDX-файлы не поддерживаются.


Выполните следующие действия для подключения и копирования данных с компьютера на диск Data Box.

1. Просмотрите содержимое разблокированного диска. Список предварительно созданных папок и вложенных папок на диске отличается в зависимости от параметров, выбранных при размещении заказа Диска Data Box.

    |Выбранное целевое хранилище  |Тип учетной записи хранения|Тип учетной записи промежуточного хранения |Папки и вложенные папки  |
    |---------|---------|---------|------------------|
    |Учетная запись хранения     |GPv1 или GPv2                 | Нет данных | BlockBlob <br> PageBlob <br> AzureFile        |
    |Учетная запись хранения     |Учетная запись хранения больших двоичных объектов         | Нет данных | BlockBlob        |
    |Управляемые диски     |Нет данных | GPv1 или GPv2         | ManagedDisk<ul> <li>PremiumSSD</li><li>StandardSSD</li><li>StandardHDD</li></ul>        |
    |Учетная запись хранения <br> Управляемые диски     |GPv1 или GPv2 | GPv1 или GPv2         |BlockBlob <br> PageBlob <br> AzureFile <br> ManagedDisk<ul> <li> PremiumSSD </li><li>StandardSSD</li><li>StandardHDD</li></ul>         |
    |Учетная запись хранения <br> Управляемые диски    |Учетная запись хранения больших двоичных объектов | GPv1 или GPv2         |BlockBlob <br> ManagedDisk<ul> <li>PremiumSSD</li><li>StandardSSD</li><li>StandardHDD</li></ul>         |

    Ниже показан пример снимка экрана заказа, в котором была указана учетная запись хранения GPv2.

    ![Содержимое диска](media/data-box-disk-deploy-copy-data/data-box-disk-content.png)
 
2. Скопируйте данные, необходимые для импорта, как блочные BLOB-объекты в папку *BlockBlob*. Аналогичным образом скопируйте данные VHD/VHDX в папку *PageBlob* и данные в папку *AzureFile*.

    Хранилище создается в учетной записи хранения Azure для каждой подпапки в папках BlockBlob и PageBlob. Все файлы в папках BlockBlob и PageBlob копируются по умолчанию в контейнер `$root` в учетную запись хранения Azure. Все файлы в контейнере `$root` всегда передаются как блочные BLOB-объекты.

   Скопируйте файлы в папку внутри папки *AzureFile*. Подпапка в папке *AzureFile* создает общую папку. Файлы, которые копируются непосредственно в папку *AzureFile*, завершатся ошибкой и будут отправлены как блочные BLOB-объекты.

    Если файлы и папки находятся в корневом каталоге, прежде чем начать копирование данных, необходимо перенести их в другую папку.

    > [!IMPORTANT]
    > Все контейнеры, большие двоичные объекты и названия файлов должны соответствовать [соглашениям об именовании Azure](data-box-disk-limits.md#azure-block-blob-page-blob-and-file-naming-conventions). Если не следовать этим правилам, произойдет сбой передачи данных в Azure.

3. При копировании файлов убедитесь, что файлы не превышают примерно 4,7 TиБ для блочных BLOB-объектов, 8 TиБ для страничных BLOB-объектов и 1 TиБ для Файлов Azure. 
4. Для копирования данных можно использовать функцию "Перетащить и отпустить" с помощью проводника. Также можно использовать любое средство SMB для копирования файлов, например Robocopy. Несколько заданий копирования могут быть инициированы с использованием следующей команды Robocopy.

    `Robocopy <source> <destination>  * /MT:64 /E /R:1 /W:1 /NFL /NDL /FFT /Log:c:\RobocopyLog.txt` 
    
    Параметры и функции для команды приведены в таблице следующим образом:
    
    |Параметры или функции  |ОПИСАНИЕ |
    |--------------------|------------|
    |Источник            | Указание пути к исходному каталогу.        |
    |Место назначения       | Указание пути к целевому каталогу.        |
    |/E                  | Копирование подкаталогов, включая пустые каталоги. |
    |/MT [: N]             | Создание многопоточных копий с N потоками, где N — целое число от 1 до 128. <br>Значение по умолчанию для N — 8.        |
    |/R: \<N>             | Указание количества повторных попыток для неудавшихся копий. Значение по умолчанию N составляет 1 000 000 (один миллион попыток).        |
    |/W: \<N>             | Указание времени ожидания между повторными попытками в секундах. Значение по умолчанию N — 30 (время ожидания 30 секунд).        |
    |/NFL                | Указание, что имена файлов не должны регистрироваться.        |
    |/NDL                | Указание, что имена каталогов не должны регистрироваться.        |
    |/FFT                | Предположение времени файла FAT (двухсекундная точность).        |
    |/Log:\<файл_журнала>     | Запись выходных данных о состоянии в файл журнала (перезапись существующего файла журнала).         |

    Можно использовать несколько дисков параллельно с несколькими заданиями, выполняемыми на каждом диске.

6. Проверьте состояние копирования, когда выполняется задание. Ниже приведен пример выходных данных команды Robocopy для копирования файлов на диск Data Box.

    ```
    C:\Users>robocopy
        -------------------------------------------------------------------------------
       ROBOCOPY     ::     Robust File Copy for Windows
    -------------------------------------------------------------------------------
    
      Started : Thursday, March 8, 2018 2:34:53 PM
           Simple Usage :: ROBOCOPY source destination /MIR
    
                 source :: Source Directory (drive:\path or \\server\share\path).
            destination :: Destination Dir  (drive:\path or \\server\share\path).
                   /MIR :: Mirror a complete directory tree.
    
        For more usage information run ROBOCOPY /?    
    
    ****  /MIR can DELETE files as well as copy them !
    
    C:\Users>Robocopy C:\Git\azure-docs-pr\contributor-guide \\10.126.76.172\devicemanagertest1_AzFile\templates /MT:64 /E /R:1 /W:1 /FFT 
    -------------------------------------------------------------------------------
       ROBOCOPY     ::     Robust File Copy for Windows
    -------------------------------------------------------------------------------
    
      Started : Thursday, March 8, 2018 2:34:58 PM
       Source : C:\Git\azure-docs-pr\contributor-guide\
         Dest : \\10.126.76.172\devicemanagertest1_AzFile\templates\
    
        Files : *.*
    
      Options : *.* /DCOPY:DA /COPY:DAT /MT:8 /R:1000000 /W:30
    
    ------------------------------------------------------------------------------
    
    100%        New File                 206        C:\Git\azure-docs-pr\contributor-guide\article-metadata.md
    100%        New File                 209        C:\Git\azure-docs-pr\contributor-guide\content-channel-guidance.md
    100%        New File                 732        C:\Git\azure-docs-pr\contributor-guide\contributor-guide-index.md
    100%        New File                 199        C:\Git\azure-docs-pr\contributor-guide\contributor-guide-pr-criteria.md
                New File                 178        C:\Git\azure-docs-pr\contributor-guide\contributor-guide-pull-request-co100%  .md
                New File                 250        C:\Git\azure-docs-pr\contributor-guide\contributor-guide-pull-request-et100%  e.md
    100%        New File                 174        C:\Git\azure-docs-pr\contributor-guide\create-images-markdown.md
    100%        New File                 197        C:\Git\azure-docs-pr\contributor-guide\create-links-markdown.md
    100%        New File                 184        C:\Git\azure-docs-pr\contributor-guide\create-tables-markdown.md
    100%        New File                 208        C:\Git\azure-docs-pr\contributor-guide\custom-markdown-extensions.md
    100%        New File                 210        C:\Git\azure-docs-pr\contributor-guide\file-names-and-locations.md
    100%        New File                 234        C:\Git\azure-docs-pr\contributor-guide\git-commands-for-master.md
    100%        New File                 186        C:\Git\azure-docs-pr\contributor-guide\release-branches.md
    100%        New File                 240        C:\Git\azure-docs-pr\contributor-guide\retire-or-rename-an-article.md
    100%        New File                 215        C:\Git\azure-docs-pr\contributor-guide\style-and-voice.md
    100%        New File                 212        C:\Git\azure-docs-pr\contributor-guide\syntax-highlighting-markdown.md
    100%        New File                 207        C:\Git\azure-docs-pr\contributor-guide\tools-and-setup.md
    ------------------------------------------------------------------------------
    
                   Total    Copied   Skipped  Mismatch    FAILED    Extras
        Dirs :         1         1         1         0         0         0
       Files :        17        17         0         0         0         0
       Bytes :     3.9 k     3.9 k         0         0         0         0
       Times :   0:00:05   0:00:00                       0:00:00   0:00:00
        
       Speed :                5620 Bytes/sec.
       Speed :               0.321 MegaBytes/min.
       Ended : Thursday, March 8, 2018 2:34:59 PM
        
    C:\Users>
    ```
 
    Чтобы оптимизировать производительность, при копировании данных используйте указанные ниже параметры robocopy.

    |    платформа    |    Главным образом малые файлы (менее 512 КБ)                           |    Главным образом файлы среднего размера (от 512 КБ до 1 МБ)                      |    Главным образом большие файлы (более 1 МБ)                             |   
    |----------------|--------------------------------------------------------|--------------------------------------------------------|--------------------------------------------------------|
    |    Диск Data Box        |    4 сеанса Robocopy* <br> 16 потоков на сеансы    |    2 сеанса Robocopy* <br> 16 потоков на сеансы    |    2 сеанса Robocopy* <br> 16 потоков на сеансы    |
    
    **Каждый сеанс Robocopy может иметь не более 7000 каталогов и 150 миллионов файлов.*
    
    >[!NOTE]
    > Приведенные выше параметры основаны на среде, используемой для внутреннего тестирования.
    
    Дополнительные сведения о команде Robocopy см. в статье [Robocopy и несколько примеров](https://social.technet.microsoft.com/wiki/contents/articles/1073.robocopy-and-a-few-examples.aspx).

6. Откройте папку назначения для просмотра и проверки скопированных файлов. При наличии ошибок во время копирования скачайте файлы журналов для устранения неполадок. Файлы журналов располагаются так, как указано в команде Robocopy.
 
### <a name="split-and-copy-data-to-disks"></a>Разделение данных и их копирование на диски

Эту дополнительную процедуру можно применять при использовании нескольких дисков и при наличии большого набора данных, который нужно разделить и скопировать на все диски. Инструмент Data Box Split Copy позволяет разделить и скопировать данные на компьютере с Windows.

>[!IMPORTANT]
> Кроме того, инструмент Data Box Split Copy проверяет ваши данные. Во время использования инструмента Data Box Split Copy для копирования данных вы можете пропустить шаг [Проверка данных](#validate-data).
> Управляемые диски не поддерживают средство Split Copy.

1. Скачайте инструмент Data Box Split Copy на компьютер с Windows и извлеките его в локальную папку. Этот инструмент был скачан при скачивании набора инструментов Диска Data Box для Windows.
2. Откройте проводник. Запишите диск источника данных и буквы диска, назначенного Диску Data Box. 

     ![Разбиение и копирование данных](media/data-box-disk-deploy-copy-data/split-copy-1.png)
 
3. Определите исходные данные для копирования. Так, в нашем примере:
    - Были определены следующие данные блочного BLOB-объекта.

         ![Разбиение и копирование данных](media/data-box-disk-deploy-copy-data/split-copy-2.png)    

    - Были определены следующие данные страничного BLOB-объекта.

         ![Разбиение и копирование данных](media/data-box-disk-deploy-copy-data/split-copy-3.png)
 
4. Перейдите в папку, в которой извлекается программное обеспечение. Найдите файл `SampleConfig.json` в каталоге. Это файл только для чтения, который можно изменить и сохранить.

   ![Разбиение и копирование данных](media/data-box-disk-deploy-copy-data/split-copy-4.png)
 
5. Измените файл `SampleConfig.json`.
 
   - Укажите имя задания. На Диске Data Box будет создана папка, которая в конечном итоге станет контейнером в учетной записи хранения Azure, связанной с этими дисками. Имя задания должно соответствовать соглашениям об именовании контейнеров Azure. 
   - Укажите путь источника, записав формат пути в файле `SampleConfigFile.json`. 
   - Введите буквы дисков, соответствующие целевым дискам. Данные берутся из исходного пути и копируются на несколько дисков.
   - Укажите путь для файлов журналов. По умолчанию он отправляется в текущий каталог, в котором находится `.exe`.

     ![Разбиение и копирование данных](media/data-box-disk-deploy-copy-data/split-copy-5.png)

6. Чтобы проверить формат этого файла, перейдите в `JSONlint`. Сохраните файл как `ConfigFile.json`. 

     ![Разбиение и копирование данных](media/data-box-disk-deploy-copy-data/split-copy-6.png)
 
7. Откройте окно командной строки и 

8. Запустите `DataBoxDiskSplitCopy.exe`. type

    `DataBoxDiskSplitCopy.exe PrepImport /config:<Your-config-file-name.json>`

     ![Разбиение и копирование данных](media/data-box-disk-deploy-copy-data/split-copy-7.png)
 
9. Чтобы продолжить выполнение сценария, введите следующее:

    ![Разбиение и копирование данных](media/data-box-disk-deploy-copy-data/split-copy-8.png)
  
10. Когда набор данных разделен и скопирован, отображается сводка инструмента Split Copy для сеанса копирования. Результат выполнения команды показан ниже.

    ![Разбиение и копирование данных](media/data-box-disk-deploy-copy-data/split-copy-9.png)
 
11. Убедитесь, что данные на целевых дисках разделены. 
 
    ![Разбиение и копирование данных ](media/data-box-disk-deploy-copy-data/split-copy-10.png)
    ![Split copy data](media/data-box-disk-deploy-copy-data/split-copy-11.png)
     
    Если вы изучите содержимое диска `n:`, вы увидите, что созданы две подпапки, соответствующие данным формата блочного и страничного BLOB-объектов.
    
     ![Разбиение и копирование данных](media/data-box-disk-deploy-copy-data/split-copy-12.png)

12. Если сеанс копирования завершается ошибкой, для восстановления используйте следующую команду:

    `DataBoxDiskSplitCopy.exe PrepImport /config:<configFile.json> /ResumeSession`

Если при работе со средством разбиения и копирования возникают ошибки, см. статью [Troubleshoot data copy issues in Azure Data Box Disk](data-box-disk-troubleshoot-data-copy.md) (Устранение неполадок копирования данных на Диске Azure Data Box).

После завершения копирования данных вы можете приступить к проверке ваших данных. Если вы использовали инструмент Split Copy, пропустите проверку (инструмент Split Copy выполняет проверку) и перейдите к следующему руководству.


## <a name="validate-data"></a>Проверка данных

Если вы не использовали инструмент Split Copy для копирования данных, вам будет необходимо проверить данные. Чтобы проверить данные, выполните описанные ниже действия.

1. Запустите `DataBoxDiskValidation.cmd` для проверки контрольных сумм в папке *DataBoxDiskImport* на диске.
    
    ![Выходные данные инструмента проверки диска Data Box](media/data-box-disk-deploy-copy-data/data-box-disk-validation-tool-output.png)

2. Выберите подходящий вариант. **Мы рекомендуем всегда проверять файлы и создавать контрольные суммы с помощью варианта 2**. В зависимости от размера данных, это действие может занять некоторое время. После выполнения сценария выйдите из командного окна. При возникновении ошибок во время проверки и создания контрольной суммы вы получите уведомление и ссылку на журналы ошибок.

    ![Выходные Checksum](media/data-box-disk-deploy-copy-data/data-box-disk-checksum-output.png)

    > [!TIP]
    > - Сбросьте инструмент между запусками.
    > - Используйте вариант 1 для файлов, которые работают с большим набором данных, содержащим небольшие файлы (~КБ). Этот вариант применим только к таким файлам, поскольку создание контрольной суммы может занять много времени и производительность может быть снижена.

3. Если используется несколько дисков, выполните команду для каждого диска.

Если возникают ошибки при проверке, см. статью [Use logs to troubleshoot validation issues in Azure Data Box Disk](data-box-disk-troubleshoot.md) (Устранение неполадок проверки на Диске Azure Data Box с помощью журналов).

## <a name="next-steps"></a>Дополнительная информация

В этом руководстве раскрыты следующие сведения о диске Azure Data Box.

> [!div class="checklist"]
> * Копирование данных на диск Data Box
> * Проверка целостности данных

Перейдите к следующему руководству, чтобы узнать, как возвратить диск Data Box и проверить отправку данных в Azure.

> [!div class="nextstepaction"]
> [Return Azure Data Box Disk and verify data upload to Azure](./data-box-disk-deploy-picked-up.md) (Возврат диска Azure Data Box и проверка загрузки данных в Azure)

::: zone-end

::: zone target="chromeless"

## <a name="copy-data-to-disks"></a>Копирование данных на диски

Выполните следующие действия для подключения и копирования данных с компьютера на диск Data Box.

1. Просмотрите содержимое разблокированного диска. Список предварительно созданных папок и вложенных папок на диске отличается в зависимости от параметров, выбранных при размещении заказа Диска Data Box.
2. Скопируйте данные в папки, предназначенные для используемого вами формата данных. Например, скопируйте неструктурированные данные в папку для папки *BlockBlob*, данные файлов VHD или VHDX в папку *PageBlob*, а файлы — в *AzureFile*. Если формат данных не соответствует соответствующей папке (типу хранилища), то на более позднем этапе загрузка данных в Azure завершится с ошибкой.

    - Хранилище создается в учетной записи хранения Azure для каждой подпапки в папках BlockBlob и PageBlob. Все файлы в папках *BlockBlob* и *PageBlob* копируются по умолчанию в контейнер $root в учетную запись хранения Azure. 
    - Все файлы в контейнере $root всегда передаются как блочные BLOB-объекты.
    - Скопируйте файлы в папку внутри папки *AzureFile*. Подпапка в папке *AzureFile* создает общую папку. Файлы, которые копируются непосредственно в папку *AzureFile*, завершатся ошибкой и будут отправлены как блочные BLOB-объекты.
    - Если файлы и папки находятся в корневом каталоге, прежде чем начать копирование данных, необходимо перенести их в другую папку.
    - Если в заказе управляемые диски являются одним из назначений хранилища, см. Соглашения об именовании [управляемых дисков](data-box-disk-limits.md#managed-disk-naming-conventions).

    > [!IMPORTANT]
    > Все контейнеры, BLOB-объекты и файлы должны соответствовать [соглашениям об именовании Azure](data-box-disk-limits.md#azure-block-blob-page-blob-and-file-naming-conventions) и [ограничениям размера для объектов Azure](data-box-disk-limits.md#azure-object-size-limits). Если не следовать этим правилам или ограничениям, произойдет сбой передачи данных в Azure.

3. Перетащите файл с помощью проводника или любого средства для копирования файлов, совместимого с протоколом SMB, например Robocopy. Несколько заданий копирования могут быть инициированы с использованием следующей команды.

    ```
    Robocopy <source> <destination>  * /MT:64 /E /R:1 /W:1 /NFL /NDL /FFT /Log:c:\RobocopyLog.txt
    ```
4. Откройте папку назначения для просмотра и проверки скопированных файлов. При наличии ошибок во время копирования скачайте файлы журналов для устранения неполадок. Файлы журналов располагаются так, как указано в команде Robocopy.

Эту дополнительную процедуру [разделения и копирования](data-box-disk-deploy-copy-data.md#split-and-copy-data-to-disks) можно применять при использовании нескольких дисков и при наличии большого набора данных, который нужно разделить и скопировать на все диски.

## <a name="validate-data"></a>Проверка данных

Выполните следующие действия, чтобы проверить данные.

1. Запустите `DataBoxDiskValidation.cmd` для проверки контрольных сумм в папке *DataBoxDiskImport* на диске.
2. Используйте параметр 2, чтобы проверить файлы и создать контрольные суммы. В зависимости от размера данных, это действие может занять некоторое время. При возникновении ошибок во время проверки и создания контрольной суммы вы получите уведомление и ссылку на журналы ошибок.

    Если возникают ошибки при проверке, см. статью [Use logs to troubleshoot validation issues in Azure Data Box Disk](data-box-disk-troubleshoot.md) (Устранение неполадок проверки на Диске Azure Data Box с помощью журналов).

::: zone-end
