---
title: Создание Устойчивых функций с помощью портала Azure
description: Сведения о том, как установить расширение Устойчивых функций для Функций Azure для разработки с помощью портала.
services: functions
author: ggailey777
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 10/23/2018
ms.author: glenga
ms.reviewer: azfuncdf
ms.openlocfilehash: 4670dd37facf341e355d736a72b4d71f27237fda
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/07/2019
ms.locfileid: "67612901"
---
# <a name="create-durable-functions-using-the-azure-portal"></a>Создание Устойчивых функций с помощью портала Azure

Расширение [устойчивых функций](durable-functions-overview.md) для Функций Azure предоставляется в пакете NuGet [Microsoft.Azure.WebJobs.Extensions.DurableTask](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.DurableTask). Это расширение необходимо установить в приложении-функции. В этой статье показано, как установить этот пакет, чтобы иметь возможность разрабатывать Устойчивые функции на портале Azure.

> [!NOTE]
> 
> * Если вы разрабатываете устойчивых функций в C#, следует рассматривать [разработки Visual Studio 2019](durable-functions-create-first-csharp.md).
> * Если вы разрабатываете Устойчивые функции на языке JavaScript, вместо этого рассмотрите возможность [разработки с помощью Visual Studio Code](./quickstart-js-vscode.md).

## <a name="create-a-function-app"></a>Создание приложения-функции

Для выполнения любых функций вам понадобится приложение-функция, позволяющее группировать функции в логические единицы и упростить развертывание и совместное использование ресурсов, а также управление ими. Вы можете создать приложение .NET или JavaScript.

[!INCLUDE [Create function app Azure portal](../../../includes/functions-create-function-app-portal.md)]

По умолчанию для приложения-функции используется версия 2.x среды выполнения Функций Azure. Расширение Устойчивых функций работает для обеих версий 1.x и 2.x среды выполнения Функций Azure на C# и версии 2.x на JavaScript. Но шаблоны доступны, только если планируется использовать среду выполнения версии 2.x., независимо от выбранного языка.

## <a name="install-the-durable-functions-npm-package-javascript-only"></a>Установка пакета npm для расширения "Устойчивые функции" (только JavaScript)

Если вы создаете устойчивые функции на JavaScript, необходимо установить [пакет npm `durable-functions`](https://www.npmjs.com/package/durable-functions).

1. Выберите имя приложения-функции, а затем — **Функции платформы** и **Дополнительные инструменты (Kudu)** .

   ![Выбор Функций, элемента "Функции платформы" и "Дополнительные инструменты (Kudu)"](./media/durable-functions-create-portal/function-app-platform-features-choose-kudu.png)

2. В консоли Kudu выберите **Консоль отладки**, а затем — **CMD**.

   ![Консоль отладки Kudu](./media/durable-functions-create-portal/kudu-choose-debug-console.png)

3. Появится структура каталогов файлов приложения-функции. Перейдите в папку `site/wwwroot`. Из нее вы можете отправить файл `package.json`, перетащив его в окно каталогов файлов. Ниже приведен пример файла `package.json`.

    ```json
    {
      "dependencies": {
        "durable-functions": "^1.1.2"
      }
    }
    ```

   ![Отправка файла package.json в Kudu](./media/durable-functions-create-portal/kudu-choose-debug-console.png)

4. Когда файл `package.json` будет передан, запустите команду `npm install` в консоли удаленного выполнения Kudu.

   ![Запуск команды npm install в Kudu](./media/durable-functions-create-portal/kudu-npm-install.png)

## <a name="create-an-orchestrator-function"></a>Создание функции оркестратора

1. Разверните приложение-функцию и нажмите кнопку **+** рядом с элементом **Функции**. Если это первая функция в приложении-функции, последовательно выберите **На портале** и **Продолжить**. Если нет, перейдите к шагу 3.

   ![Страница быстрого начала работы с функциями на портале Azure](./media/durable-functions-create-portal/function-app-quickstart-choose-portal.png)

1. Последовательно выберите **Дополнительные шаблоны** и **Finish and view templates** (Закончить и просмотреть шаблоны).

    ![Быстрый запуск функций: просмотр дополнительных шаблонов](./media/durable-functions-create-portal/add-first-function.png)

1. В поле поиска введите `durable` и выберите шаблон **Начальный объект HTTP устойчивых функций**.

1. Когда появится соответствующий запрос, щелкните **Установить**, чтобы установить в приложение-функцию расширение Azure DurableTask и другие необходимые зависимости. Установить расширение для приложения-функции необходимо всего один раз. По завершении установки щелкните **Продолжить**.

    ![Установка расширений привязки](./media/durable-functions-create-portal/install-durabletask-extension.png)

1. После завершения установки присвойте новой функции имя `HttpStart` и выберите **Создать**. Созданная функция используется для запуска оркестрации.

1. Создайте другую функцию в приложении-функции, но на этот раз используя шаблон **Оркестратор устойчивых функций**. Присвойте новой функции оркестрации имя `HelloSequence`.

1. Создайте третью функцию с именем `Hello`, используя шаблон **Действие устойчивых функций**.

## <a name="test-the-durable-function-orchestration"></a>Тестирование оркестрации устойчивых функций

1. Вернитесь к функции **HttpStart**, выберите **</> Получить URL-адрес функции** и щелкните **Копировать**, чтобы скопировать URL-адрес. С помощью этого URL-адреса можно запустить функцию **HelloSequence**.

1. Используя средство HTTP, например Postman или cURL, отправьте запрос POST к скопированному URL-адресу. В следующем примере представлена команда cURL, которая отправляет запрос POST к устойчивой функции:

    ```bash
    curl -X POST https://{your-function-app-name}.azurewebsites.net/api/orchestrators/HelloSequence
    ```

    В этом примере `{your-function-app-name}` — это домен, который является именем приложения-функции. Ответное сообщение содержит набор конечных точек URI, с помощью которых можно отслеживать и контролировать выполнение, которое выглядит следующим образом:

    ```json
    {  
       "id":"10585834a930427195479de25e0b952d",
       "statusQueryGetUri":"https://...",
       "sendEventPostUri":"https://...",
       "terminatePostUri":"https://...",
       "rewindPostUri":"https://..."
    }
    ```

1. Вызовите URI конечной точки `statusQueryGetUri`, и вы увидите текущее состояние устойчивой функции, которое может выглядеть следующим образом:

    ```json
        {
            "runtimeStatus": "Running",
            "input": null,
            "output": null,
            "createdTime": "2017-12-01T05:37:33Z",
            "lastUpdatedTime": "2017-12-01T05:37:36Z"
        }
    ```

1. Продолжайте вызывать конечную точку `statusQueryGetUri`, пока состояние не изменится на **Завершено**, и вы увидите ответ, подобный следующему:

    ```json
    {
            "runtimeStatus": "Completed",
            "input": null,
            "output": [
                "Hello Tokyo!",
                "Hello Seattle!",
                "Hello London!"
            ],
            "createdTime": "2017-12-01T05:38:22Z",
            "lastUpdatedTime": "2017-12-01T05:38:28Z"
        }
    ```

Ваша первая устойчивая функция создана и успешно работает в Azure.

## <a name="next-steps"></a>Следующие шаги

> [!div class="nextstepaction"]
> [Дополнительные сведения о распространенных шаблонах устойчивых функций](durable-functions-concepts.md)
