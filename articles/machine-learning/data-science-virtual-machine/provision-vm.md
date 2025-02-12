---
title: Создание виртуальной машины Windows для обработки и анализа данных
titleSuffix: Azure
description: Настройка и создание виртуальной машины в Azure для обработки и анализа данных и машинного обучения.
services: machine-learning
documentationcenter: ''
author: gopitk
manager: cgronlun
ms.custom: seodec18
ms.assetid: e1467c0f-497b-48f7-96a0-7f806a7bec0b
ms.service: machine-learning
ms.subservice: data-science-vm
ms.workload: data-services
ms.devlang: na
ms.topic: article
ms.date: 02/22/2019
ms.author: gokuma
ms.openlocfilehash: 57768fc463f5ea1d2f1ec386f3f0975758220013
ms.sourcegitcommit: c0419208061b2b5579f6e16f78d9d45513bb7bbc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/08/2019
ms.locfileid: "67626385"
---
# <a name="provision-a-windows-data-science-virtual-machine-on-azure"></a>Подготовка Виртуальной машины Windows для обработки и анализа данных в Azure

Виртуальная машина Microsoft Windows для обработки и анализа данных (DSVM) — это образ виртуальной машины Windows Server 2016 в Azure, на которой предварительно установлены и настроены средства анализа данных и машинного обучения.

## <a name="included-data-science-tools"></a>Включенные средства обработки и анализа данных

На виртуальной машине DSVM доступны следующие средства:

* Пакет Python SDK для [службы "Машинное обучение Azure"](../service/index.yml).
* [Microsoft Machine Learning Server](https://docs.microsoft.com/machine-learning-server/index) Developer Edition.
* дистрибутив Anaconda Python;
* Jupyter Notebook (с ядрами R, Python и PySpark).
* Microsoft Visual Studio Community.
* Microsoft Power BI Desktop.
* Microsoft SQL Server 2017 Developer Edition.
* Автономный экземпляр Apache Spark для локальной разработки и тестирования.
* [JuliaPro](https://juliacomputing.com/products/juliapro.html);
* Средства машинного обучения и анализа данных:
  * Платформы глубокого обучения. Виртуальная машина содержит функциональный набор платформ искусственного интеллекта: [Microsoft Cognitive Toolkit](https://www.microsoft.com/en-us/cognitive-toolkit/), [TensorFlow](https://www.tensorflow.org/), [Chainer](https://chainer.org/), mxNet и Keras.
  * [Vowpal Wabbit.](https://github.com/JohnLangford/vowpal_wabbit) Система быстрого машинного обучения, поддерживающая такие методы, как онлайн-обучение, хэширование, общее сокращение, сокращение, обучение поиску, активное и интерактивное обучение.
  * [XGBoost. ](https://xgboost.readthedocs.org/en/latest/)Инструмент, предоставляющий быструю и точную реализацию усиленного дерева.
  * [Rattle.](https://togaware.com/rattle/) Аналитическое средство, которое помогает приступить к анализу данных и работе со средствами машинного обучения в R, предоставляя графический пользовательский интерфейс для исследования данных и моделирования с автоматическим созданием кода R.
  * [Weka.](https://www.cs.waikato.ac.nz/ml/weka/) Программное обеспечение для визуального интеллектуального анализа данных и машинного обучения, написанное на языке Java.
  * [Apache Drill.](https://drill.apache.org/) Обработчик SQL-запросов без схемы для Apache Hadoop, NoSQL и облачного хранилища. Он поддерживает интерфейсы ODBC и JDBC для запросов по NoSQL и файлам стандартных средств бизнес-аналитики, таких как PowerBI, Excel и Tableau.
* Библиотеки R и Python для машинного обучения Azure и других служб Azure
* Git, включая Git Bash, для работы с репозиториями исходного кода, такими как GitHub и Azure DevOps Services. Git предоставляет несколько популярных служебных программ Linux, которые можно использовать как в Git Bash, так и в командной строке. Сюда входят, например, awk, sed, perl, grep, find, wget и curl.

### <a name="about-data-science"></a>Сведение об обработке и анализе данных

При обработке и анализе данных выполняются следующие задачи:

1. Поиск, загрузка и предварительная обработка данных.
1. Сборка и тестирование моделей.
1. Развертывание моделей для использования в интеллектуальных приложениях.

Специалисты по анализу данных используют для этих задач несколько средств. Поиск, скачивание и установка нужной версии программного обеспечения иногда занимают много времени. Виртуальная машина DSVM позволяет не тратить время на выполнение этих задач, предоставляя готовый образ, который можно подготовить в Azure. В этом образе уже установлены и настроены многие популярные инструменты.

Используйте ее, чтобы быстро начать работу со своим аналитическим проектом. Вы сможете работать над задачами на разных языках, включая R, Python, SQL и C#. Visual Studio предоставляет простую платформу IDE (интегрированную среду разработки) для разработки и тестирования кода. Пакет SDK Azure в виртуальной машине позволяет создавать приложения с помощью различных служб на облачной платформе Майкрософт.

Плата за программное обеспечение для этого образа виртуальной машины не взимается. Вы платите только за использование Azure. Суммы оплаты зависят от размера подготовленной виртуальной машины. Дополнительные сведения об оплате вычислительных ресурсов см. в **этом разделе** на странице [Data Science Virtual Machine](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-dsvm.dsvm-windows) (Виртуальная машина для обработки и анализа данных).

### <a name="other-dsvm-versions"></a>Другие версии DSVM

* Образ [Ubuntu](dsvm-ubuntu-intro.md). В нем есть множество средств, как и в DSVM, а также несколько дополнительных платформ глубокого обучения.
* Образ [Linux CentOS](linux-dsvm-intro.md).
* Выпуск [Windows Server 2012](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-dsvm.dsvm-windows) Виртуальной машины для обработки и анализа данных. Некоторые средства доступны только в выпуске Windows Server 2016. За исключением этого, сведения в настоящей статье также применимы к выпуску Windows Server 2012.

## <a name="prerequisite"></a>Предварительные требования

Чтобы создать Виртуальную машину Майкрософт для обработки и анализа данных, вам потребуется подписка Azure. См. страницу [бесплатной пробной версии Azure](https://azure.com/free).

## <a name="create-your-dsvm"></a>Создание виртуальной машины DSVM

Чтобы создать экземпляр виртуальной машины DSVM, сделайте следующее:

1. Перейдите к списку виртуальных машин на [портале Azure](https://portal.azure.com/#create/microsoft-dsvm.dsvm-windowsserver-2016). Если вы еще не вошли в свою учетную запись Azure, появится запрос на вход.
1. В нижней части страницы нажмите кнопку **Создать**.

   ![configure-data-science-vm](./media/provision-vm/configure-data-science-virtual-machine.png)

1. Чтобы настроить все этапы, представленные в правой части снимка экрана, необходимо ввести следующие данные:

   1. **Основные сведения**:
      * **Имя.** Имя виртуальной машины DSVM.
      * **Тип диска виртуальной машины.** Выберите **SSD** или **HDD**. Для экземпляра GPU NC_v1 (например, на основе NVidia Tesla K80) выберите тип диска **HDD**.
      * **Имя пользователя.** Идентификатор учетной записи администратора.
      * **Пароль.** Пароль учетной записи администратора.
      * **Подписка**: Если у вас есть несколько подписок, выберите ту, в которой будет создана виртуальная машина и для которой будут выставляться счета.
      * **Группа ресурсов**: выберите существующую группу ресурсов Azure или создайте новую группу. Вы можете создать новую или использовать существующую группу.
      * **Расположение.** Выберите наиболее подходящий центр обработки данных. Чтобы ускорить доступ к сети, выбирайте центр обработки данных, в котором размещена большая часть ваших данных, или который ближе всего вашему физическому расположению.
   1. **Размер**: Выберите один из типов серверов, который соответствует вашим функциональным требованиям и финансовым ограничениям. Выберите **Просмотреть все**, чтобы отобразились дополнительные варианты размеров виртуальных машин.
   1. **Параметры**:  
      * **Использовать управляемые диски.** Выберите **Управляемый**, если вы хотите доверить управление дисками для виртуальной машины платформе Azure. Если нет, укажите новую или существующую учетную запись хранения.  
      * **Другие параметры.** Вы можете сохранить все значения по умолчанию. Если вы хотите изменить эти значения, наведите указатель мыши на информационную ссылку, чтобы получить справку по конкретному полю.
   1. **Сводка**: Убедитесь, что все сведения введены правильно. Нажмите кнопку **Создать**.

> [!NOTE]
> * За использование виртуальной машины не взимается дополнительная плата, кроме платы за размер сервера, выбранный на шаге **Размер**.
> * Подготовка занимает примерно 10–20 минут. Состояние виртуальной машины отображается на портале Azure.

## <a name="how-to-access-the-dsvm"></a>Доступ к виртуальной машине DSVM

После создания и подготовки виртуальной машины вы можете подключиться к ней по протоколу удаленного рабочего стола, используя учетную запись администратора, настроенную в разделе **Основные сведения**. Теперь все готово к использованию установленных и настроенных на виртуальной машине средств. Ко многим средствам можно получить доступ через значки на рабочем столе и элементы в меню "Пуск".

Также можно присоединить виртуальную Машину обработки и анализа данных для записных книжек Azure запускать записные книжки Jupyter на виртуальной Машине и обойти ограничения на уровне бесплатная служба. Дополнительные сведения см. в разделе [управление и Настройка проектов записные книжки - уровня вычислений](/azure/notebooks/configure-manage-azure-notebooks-projects#compute-tier).

## <a name="tools-installed-on-the-microsoft-data-science-virtual-machine"></a>Средства, установленные на виртуальной машине Майкрософт для обработки и анализа данных

Ниже приведены сведения о средствах, установленных на виртуальных машинах DSVM.

### <a name="microsoft-machine-learning-server-developer-edition"></a>Microsoft Machine Learning Server Developer Edition

Вы можете использовать для аналитических задач библиотеку Microsoft Enterprise Library, чтобы создавать масштабируемый код на R или Python, так как на виртуальной машине установлен выпуск Machine Learning Server Developer Edition. Это широко используемая аналитическая платформа корпоративного класса, ранее известная как Microsoft R Server. Она работает с R и Python, хорошо масштабируется, безопасна и имеет коммерческую поддержку.

Machine Learning Server поддерживает разнообразную статистику для больших данных, прогнозное моделирование и машинное обучение. Для вас будет доступен полный спектр аналитических задач: исследование, анализ, визуализация и моделирование. Machine Learning Server использует расширенную поддержку открытого кода на R и Python. Платформа полностью совместима со скриптами и функциями R и Python. Также обеспечена совместимость с пакетами CRAN, pip и Conda для анализа данных на корпоративном уровне.

Machine Learning Server обходит ограничения R по обработке кода в памяти, реализуя параллельную и фрагментарную обработку данных. Это позволяет выполнять анализ данных, размер которых намного превышает объем доступной памяти. Виртуальная машина содержит также Visual Studio Community. Этот инструмент содержит средства Python для Visual Studio и расширение "Инструменты Python для Visual Studio" (PTVS), которые предоставляют полную среду IDE для работы с R или Python. Кроме того, на виртуальной машине доступны другие среды IDE, например [RStudio](https://www.rstudio.com) и [выпуск PyCharm Community](https://www.jetbrains.com/pycharm/).

### <a name="python"></a>Python

Для разработки на языке Python на виртуальной машине установлены дистрибутивы Anaconda Python версий 2.7 и 3.6. Эти дистрибутивы содержат базовую версию Python, а также примерно 300 наиболее популярных математических и инженерных пакетов и пакетов аналитики данных. Вы можете использовать подключаемый модуль PTVS, который устанавливается в Visual Studio Community 2017. Также можно использовать любую из сред IDE, включенную в комплектацию Anaconda, например IDLE или Spyder. Найдите и запустите один из этих пакетов (Win+S).

> [!NOTE]
> Чтобы Инструменты Python для Visual Studio указывали на Anaconda Python 2.7, необходимо создать пользовательские среды для каждой версии. Чтобы настроить пути в выпуске Visual Studio 2017 Community, последовательно выберите **Инструменты** > **Инструменты Python** > **Окружения Python**. Затем выберите **+ Настраиваемое**.

Anaconda Python 3.6 устанавливается в каталог **C:\Anaconda**. Anaconda Python 2.7 устанавливается в каталог **C:\Anaconda\envs\python2**. Подробные инструкции есть в [документации по PTVS](https://docs.microsoft.com/visualstudio/python/installing-python-interpreters).

### <a name="the-jupyter-notebook"></a>Jupyter Notebook

Дистрибутив Anaconda также содержит среду Jupyter Notebook для совместного использования кода и анализа. На сервере Jupyter Notebook предварительно настроены ядра Python 2.7, Python 3.x, PySpark, Julia и R. Чтобы запустить сервер Jupyter и браузер для доступа к серверу записной книжки, используйте значок **Записная книжка Jupyter** на рабочем столе.

Мы добавили несколько примеров записных книжек на Python и R. Когда вы откроете Jupyter, они подскажут вам, как работать со следующими технологиями:

* сервер машинного обучения;
* Службы машинного обучения SQL Server, аналитика в базах данных;
* Python
* Microsoft Cognitive Toolkit;
* Тensorflow;
* другие технологии Azure.

Ссылки на эти примеры вы увидите на домашней странице записной книжки после входа в Jupyter Notebook с использованием пароля, созданного на предыдущем шаге.

### <a name="visual-studio-community-2017"></a>Visual Studio Community 2017

На виртуальной машине DSVM установлена платформа Visual Studio Community. Это бесплатная версия популярной среды IDE корпорации Майкрософт, которую можно использовать для ознакомления и работы в небольших группах. Изучите [условия лицензирования](https://www.visualstudio.com/support/legal/mt171547).

Откройте Visual Studio с помощью значка на рабочем столе или соответствующего пункта в меню **Пуск**. Откройте окно поиска программ (Win + S) и введите **Visual Studio**. Здесь вы сможете создавать проекты на разных языках, например C#, Python, R и Node.js. Установленные подключаемые модули поддерживают работу со следующими службами Azure:

* Каталог данных Azure
* Azure HDInsight Hadoop и Spark;
* Azure Data Lake;

Еще один модуль с именем **Azure Machine Learning for Visual Studio Code** легко интегрируется со службой "Машинное обучение Azure" и позволяет быстро создавать приложения ИИ.

> [!NOTE]
> Может появиться сообщение о том, что период ознакомления завершился. Введите учетные данные для учетной записи Майкрософт. Вы можете создать новую бесплатную учетную запись, чтобы получить доступ к Visual Studio Community.

### <a name="sql-server-2017-developer-edition"></a>SQL Server 2017 Developer Edition

На виртуальной машине DSVM предоставляется версия SQL Server 2017 для разработчиков со Службами машинного обучения. Этот выпуск позволяет выполнять аналитику в базе данных с использованием R или Python. Службы машинного обучения предоставляют платформу для разработки и развертывания аналитических приложений. Вы можете воспользоваться этими языками и разнообразными пакетами, которые предоставляет сообщество, чтобы создавать модели и формировать прогнозы на основе данных из SQL Server. Так как Службы машинного обучения (в базе данных) позволяют применять языки R и Python в SQL Server, вы сможете хранить данные и аналитические сведения вместе. Такая интеграция сокращает затраты и снижает риски безопасности, связанные с перемещением данных.

> [!NOTE]
> Выпуск для разработчиков SQL Server предназначен только для разработки и тестирования. Для его использования в рабочей среде требуется лицензия.

Для доступа к серверу SQL Server запустите Microsoft SQL Server Management Studio. Имя виртуальной машины совпадает с **именем сервера**. При входе от имени администратора в ОС Windows используйте проверку подлинности Windows. Выполнив вход в SQL Server Management Studio, вы можете создавать других пользователей и базы данных, импортировать данные и выполнять запросы SQL.

Чтобы включить внутреннюю аналитику базы данных с использованием Служб машинного обучения SQL, войдите в систему с правами администратора сервера и однократно выполните в SQL Server Management Studio следующую команду:

```
CREATE LOGIN [%COMPUTERNAME%\SQLRUserGroup] FROM WINDOWS
```

Замените `%COMPUTERNAME%` на имя своей виртуальной машины.

### <a name="azure"></a>Azure

На виртуальной машине установлено несколько инструментов Azure.

* Ярлык на рабочем столе позволяет открыть документацию по пакету SDK Azure.
* **AzCopy** позволяет копировать данные в учетную запись хранения Azure и из нее. Чтобы получить сведения об использовании, введите **Azcopy** в командной строке.
* **Обозреватель службы хранилища Azure** позволяет просматривать объекты, которые хранятся в учетной записи службы хранилища Azure. Он также позволяет копировать данные в службу хранилища Azure и из нее. Чтобы открыть это средство, введите строку **обозреватель хранилища** в поле **Поиск**. Кроме того, вы можете найти соответствующий элемент в меню **Пуск** ОС Windows.
* **Adlcopy** позволяет копировать данные в Azure Data Lake. Чтобы ознакомиться со сведениями об использовании, введите **adlcopy** в командной строке.
* **dtui** позволяет копировать данные в Azure Cosmos DB (облачную базу данных NoSQL) и обратно. Введите **dtui** в командной строке.
* **Integration Runtime в Фабрике данных Azure** позволяет копировать данные между локальными источниками данных и облаком. Эта функция используется в разных инструментах, например в Фабрике данных Azure.
* **Microsoft Azure PowerShell** позволяет управлять ресурсами Azure на языке сценариев PowerShell. Оно также установлено на вашей виртуальной машине.

### <a name="power-bi"></a>Power BI

На виртуальной машине DSVM установлено приложение **Power BI Desktop**, которое позволяет создавать панели мониторинга и визуализации. Используйте это средство для извлечения данных из различных источников, создания панелей мониторинга и отчетов и их публикации в облаке. Дополнительные сведения см. на сайте [Power BI](https://powerbi.microsoft.com). Power BI Desktop можно найти в меню **Пуск**.

> [!NOTE]
> Для доступа к Power BI необходима учетная запись Microsoft Office 365.

### <a name="azure-machine-learning-service-python-sdk"></a>Пакет Python SDK для службы "Машинное обучение Azure"

Используя пакет Python SDK для службы "Машинное обучение Azure", специалисты по обработке и анализу данных и разработчики ИИ создают и запускают рабочие процессы машинного обучения с помощью [службы "Машинное обучение Azure"](../service/overview-what-is-azure-ml.md). Вы можете взаимодействовать со службой в Jupyter Notebook или любой другой интегрированной среде разработки Python, используя платформы с открытым исходным кодом, например TensorFlow и scikit-learn.

Сведения о начале работы с пакетом SDK Python см. в разделе [Начало работы с машинным обучением Azure с помощью Python](../service/quickstart-create-workspace-with-python.md).

Пакет SDK Python предустановлен в виртуальной машине для обработки и анализа данных Microsoft.

## <a name="more-microsoft-development-tools"></a>Дополнительные средства разработки Майкрософт

Для поиска и скачивания других инструментов разработки Майкрософт вы можете использовать [установщик веб-платформы Майкрософт](https://www.microsoft.com/web/downloads/platform.aspx). Ярлык этого средства размещен на рабочем столе Виртуальной машины Майкрософт для обработки и анализа данных.  

## <a name="important-directories-on-the-vm"></a>Важные каталоги на виртуальной машине

| Элемент | Каталог |
| --- | --- |
| Конфигурации сервера Jupyter Notebook | C:\ProgramData\jupyter |
| Корневой каталог примеров записных книжек Jupyter | C:\dsvm\notebooks and c:\users\\<имя_пользователя\>\notebooks |
| Другие примеры | C:\dsvm\samples |
| Anaconda, по умолчанию: Python 3.6; | C:\Anaconda |
| Среда Anaconda Python 2.7 | C:\Anaconda\envs\python2 |
| Microsoft Machine Learning Server (автономная версия) с Python | C:\Program Files\Microsoft\ML Server\PYTHON_SERVER |
| Экземпляр R по умолчанию (автономная версия Machine Learning Server) | C:\Program Files\Microsoft\ML Server\R_SERVER |
| Каталог для экземпляра Служб машинного обучения SQL в базе данных | C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER |
| Прочие инструменты | C:\dsvm\tools |

> [!NOTE]
> В DSVM с выпусками Windows Server 2012 и Windows Server 2016, развернутых до марта 2018 г., по умолчанию используется среда Anaconda для Python 2.7. Среда Python 3.5 устанавливается в качестве дополнительной в папку **C:\Anaconda\envs\py35**.

## <a name="next-steps"></a>Следующие шаги

* Изучите инструменты, включенные в Виртуальную машину для обработки и анализа данных, открыв меню **Пуск**.
* Дополнительные сведения о службе машинного обучения Azure см. в статье [Что такое служба машинного обучения Azure?](../service/overview-what-is-azure-ml.md) и ознакомьтесь с доступными [краткими руководствами и учебниками](../service/index.yml).
* В проводнике перейдите к каталогу **C:\Program Files\Microsoft\ML Server\R_SERVER\library\RevoScaleR\demoScripts**, в котором размещены примеры использования библиотеки RevoScaleR в R, поддерживающей анализ данных на корпоративном уровне.  
* Ознакомьтесь со статьей [10 задач, которые можно выполнить на виртуальной машине Windows для обработки и анализа данных](https://aka.ms/dsvmtenthings).
* Изучите систематический подход к созданию комплексных аналитических решений с помощью [группового процесса обработки и анализа данных](../team-data-science-process/index.yml).
* В [коллекции решений ИИ Azure](https://gallery.cortanaintelligence.com) можно найти примеры для машинного обучения и анализа данных, в которых используется служба "Машинное обучение Azure" и связанные с ней службы данных Azure. Мы также добавили значок на рабочий стол и элемент в меню **Пуск** для этой коллекции на виртуальной машине.
