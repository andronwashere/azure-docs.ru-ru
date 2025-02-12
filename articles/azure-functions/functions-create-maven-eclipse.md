---
title: Создание приложения-функции Azure с использованием Java и Eclipse | Документация Майкрософт
description: Практическое руководство по созданию простого бессерверного HTTP-приложения с использованием Java и Eclipse и его публикации в решении "Функции Azure".
services: functions
documentationcenter: na
author: jeffhollan
manager: jpconnock
keywords: azure functions, functions, event processing, compute, serverless architecture, java
ms.service: azure-functions
ms.devlang: java
ms.topic: conceptual
ms.date: 07/01/2018
ms.author: jehollan
ms.custom: mvc, devcenter
ms.openlocfilehash: 9dcc959e51aa42fd6ef3173dba2aec8d9970deb1
ms.sourcegitcommit: 1289f956f897786090166982a8b66f708c9deea1
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/17/2019
ms.locfileid: "67154596"
---
# <a name="create-your-first-function-with-java-and-eclipse"></a>Создание первой функции с помощью Java и Eclipse 

В этой статье показано, как создать проект [бессерверной](https://azure.microsoft.com/solutions/serverless/) функции с помощью Eclipse IDE и Apache Maven, протестировать и отладить его и развернуть в решении "Функции Azure". 

<!-- TODO ![Access a Hello World function from the command line with cURL](media/functions-create-java-maven/hello-azure.png) -->

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="set-up-your-development-environment"></a>Настройка среды разработки

Для разработки приложения-функции с помощью Java и Eclipse должны быть установлены следующие компоненты:

-  [Java Developer Kit (JDK)](https://www.azul.com/downloads/zulu/) версии 8.
-  [Apache Maven](https://maven.apache.org) 3.0 или более поздней версии.
-  [Eclipse](https://www.eclipse.org/downloads/packages/) с поддержкой Java и Maven.
-  [Интерфейс командной строки Azure](https://docs.microsoft.com/cli/azure)

> [!IMPORTANT] 
> Переменной среде JAVA_HOME необходимо присвоить расположение установки JDK, чтобы завершить выполнение заданий этого краткого руководства.

Рекомендуется установить [основные инструменты решения "Функции Azure" версии 2](functions-run-local.md#v2), которые предоставляют локальную среду для выполнения и отладки Функций Azure. 

## <a name="create-a-functions-project"></a>Создание проекта Функций Azure

1. В Eclipse, выберите **файл** меню, затем выберите **New -&gt; проект Maven**. 
1. Примите значения по умолчанию в диалоговом окне **New Maven Project** (Новый проект Maven) и щелкните **Next** (Далее).
1. Выберите **Add Archetype** (Добавить архетип) и добавьте записи для [azure-functions-archetype](https://mvnrepository.com/artifact/com.microsoft.azure/azure-functions-archetype).
    - Идентификатор группы архетипа: com.microsoft.azure.
    - Идентификатор артефакта архетипа: azure-functions-archetype.
    - Версия: Используйте последнюю версию **1.22** из [центральный репозиторий](https://mvnrepository.com/artifact/com.microsoft.azure/azure-functions-archetype)
    ![Maven в Eclipse создайте](media/functions-create-first-java-eclipse/functions-create-eclipse.png)  
1. Нажмите кнопку **ОК** и нажмите кнопку **Далее** для ввода значений, таких как следующий моментальный снимок (отличное от используйте разные appName **fabrikam функция — 20170920120101928**), и со временем **Готово**.
    ![Create2 Maven в Eclipse](media/functions-create-first-java-eclipse/functions-create-eclipse2.png)  

Maven создает файлы проекта в новой папке с именем _artifactId_. Созданный код проекта представляет собой простую функцию[активации HTTP](/azure/azure-functions/functions-bindings-http-webhook), которая возвращает текст HTTP-запроса.

## <a name="run-functions-locally-in-the-ide"></a>Запуск функций в локальной среде IDE

> [!NOTE]
> Для локальных запуска и отладки функций, нужно установить [Azure Functions Core Tools версии 2](functions-run-local.md#v2).

1. Щелкните правой кнопкой мыши созданный проект, а затем выберите **Run As** (Выполнить как) и **Maven build** (Сборка Maven).
1. В диалоговом окне **Edit Configuration** (Изменение конфигурации) введите `package` в поля **Goals** (Целевые объекты) и **Name** (Имя), затем щелкните **Run** (Выполнить). В результате будет создан и упакован код функции.
1. После завершения сборки создайте другую конфигурацию запуска так, как показано выше, используя `azure-functions:run` для целевого объекта и имени. Щелкните **Run** (Выполнить), чтобы запустить функцию в интегрированной среде разработки.

Когда закончите тестировать функцию, завершите работу среды выполнения в окне консоли. Только один узел функции может быть активным и работать локально одновременно.

### <a name="debug-the-function-in-eclipse"></a>Отладка функции в Eclipse

В конфигурации **Run As** (Выполнить как), установленной на предыдущем шаге, измените `azure-functions:run` на `azure-functions:run -DenableDebug` и запустите обновленную конфигурацию, чтобы открыть приложение-функцию в режиме отладки.

Выберите меню **запуска** и откройте **Debug Configurations** (Конфигурации отладки). Выберите пункт **Remote Java Application** (Удаленное приложение Java) и создайте новое. Присвойте конфигурации имя и задайте параметры. Порт должен соответствовать порту отладки, открытому узлом функции, который по умолчанию равен `5005`. После завершения установки щелкните `Debug`, чтобы запустить отладку.

![Функции отладки в Eclipse](media/functions-create-first-java-eclipse/debug-configuration-eclipse.PNG)

Установите точки останова и проверьте объекты в функции, используя интегрированную среду разработки. По завершении остановите отладчик и запущенный узел функции. Только один узел функции может быть активным и работать локально одновременно.

## <a name="deploy-the-function-to-azure"></a>Развертывание функции для Azure

В процессе развертывания для Функций Azure используются данные учетной записи из Azure CLI. [Войдите с помощью Azure CLI](/cli/azure/authenticate-azure-cli?view=azure-cli-latest) прежде чем продолжить, используя командную строку компьютера.

```azurecli
az login
```

Разверните свой код в новом приложении-функции, используя целевой объект Maven `azure-functions:deploy` в новой конфигурации **Run As** (Выполнить как).

После завершения развертывания появится URL-адрес, который можно использовать для доступа к приложению-функции Azure:

```output
[INFO] Successfully deployed Function App with package.
[INFO] Deleting deployment package from Azure Storage...
[INFO] Successfully deleted deployment package fabrikam-function-20170920120101928.20170920143621915.zip
[INFO] Successfully deployed Function App at https://fabrikam-function-20170920120101928.azurewebsites.net
[INFO] ------------------------------------------------------------------------
```

## <a name="next-steps"></a>Дальнейшие действия

- Дополнительные сведения о разработке функции Java см. в статье [Azure Functions Java developer guide](functions-reference-java.md) (Руководство разработчика Java для Функций Azure).
- Добавьте в проект дополнительные функции с помощью различных триггеров целевого объекта Maven`azure-functions:add`.
