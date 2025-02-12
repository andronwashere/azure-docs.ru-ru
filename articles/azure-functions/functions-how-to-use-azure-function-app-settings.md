---
title: Настройка параметров приложения-функции Azure | Документация Майкрософт
description: Узнайте, как настроить параметры приложения-функции Azure.
services: ''
documentationcenter: .net
author: ggailey777
manager: jeconnoc
ms.assetid: 81eb04f8-9a27-45bb-bf24-9ab6c30d205c
ms.service: azure-functions
ms.topic: conceptual
ms.date: 03/28/2018
ms.author: glenga
ms.custom: cc996988-fb4f-47
ms.openlocfilehash: 92ca09040836dfc55a9d709b12a0ee01192d6bac
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "65957393"
---
# <a name="how-to-manage-a-function-app-in-the-azure-portal"></a>Управление приложением-функцией на портале Azure 

В функциях Azure приложение-функция предоставляет контекст выполнения для отдельных функций. Поведение приложения-функции применяется ко всем содержащимся в нем функциям. Эта статья описывает настройку приложений-функций и управление ими на портале Azure.

Для начала перейдите на [портал Azure](https://portal.azure.com) и войдите, используя свою учетную запись Azure. На панели поиска в верхней части портала введите имя приложения-функции и выберите его в списке. После выбора приложения-функции появляется следующая страница:

![Обзор приложения-функции на портале Azure](./media/functions-how-to-use-azure-function-app-settings/azure-function-app-main.png)

Вы можете перейти к все необходимое для управления приложения-функции на странице обзора, в частности **[параметры приложения](#settings)** и **[функции платформы](#platform-features)** .

## <a name="settings"></a>Параметры приложения

**Параметры приложения** вкладке поддерживает параметры, которые используются приложением-функцией.

![Параметры приложения-функции на портале Azure.](./media/functions-how-to-use-azure-function-app-settings/azure-function-app-settings-tab.png)

Эти параметры хранятся в зашифрованном виде, и необходимо выбрать **Показывать значения** чтобы увидеть значения на портале.

Чтобы добавить параметр, выберите **новый параметр приложения** и добавьте новую пару ключ значение.

[!INCLUDE [functions-environment-variables](../../includes/functions-environment-variables.md)]

При разработке приложения-функции локально, эти значения сохраняются в файл проекта local.settings.json.

## <a name="platform-features"></a>Функции платформы

![Вкладка функций платформы для приложения-функции](./media/functions-how-to-use-azure-function-app-settings/azure-function-app-features-tab.png)

Приложения-функции выполняются на платформе службы приложения Azure и обслуживаются ею. Поэтому они имеют доступ к большинству функций базовой платформы веб-хостинга Azure. Вкладка **Функции платформы** предоставляет доступ ко многим функциям платформы службы приложений, которые можно использовать в приложениях-функциях. 

> [!NOTE]
> Не все функции службы приложений доступны при выполнении приложения с планом размещения потребления.

Ниже приводится более подробное описание следующих функций службы приложений на портале Azure, которые могут пригодиться при работе с функциями:

+ [Редактор службы приложений](#editor)
+ [Console](#console)
+ [Дополнительные инструменты (Kudu)](#kudu)
+ [Варианты развертывания](#deployment)
+ [CORS](#cors)
+ [Проверка подлинности](#auth)
+ [Определение интерфейса API](#swagger)

Дополнительные сведения о работе с параметрами службы приложений см. в статье [Настройка параметров в службе приложений Azure](../app-service/configure-common.md).

### <a name="editor"></a>Редактор службы приложений

| | |
|-|-|
| ![Редактор службы приложений для приложения-функции](./media/functions-how-to-use-azure-function-app-settings/function-app-appsvc-editor.png)  | Редактор службы приложений — это расширенный редактор на портале, который можно использовать для изменения JSON-файлов конфигурации и файлов с кодом. При выборе этого параметра откроется отдельная вкладка браузера с базовым редактором. Он позволяет выполнять интеграцию с репозиторием Git, запускать и отлаживать код и изменять параметры приложения-функции. Этот редактор предоставляет расширенную среду разработки для функций по сравнению со стандартной колонкой приложения-функции.    |

![Редактор службы приложений](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-appservice-editor.png)

### <a name="console"></a>Консоль

| | |
|-|-|
| ![Консоль приложения-функции на портале Azure](./media/functions-how-to-use-azure-function-app-settings/function-app-console.png) | Консоль на портале оптимально подходит разработчикам, желающим взаимодействовать с приложением-функцией из командной строки. Стандартные команды включают создание каталогов и файлов и навигацию по ним, а также выполнение пакетных файлов и сценариев. |

![Консоль приложения-функции](./media/functions-how-to-use-azure-function-app-settings/configure-function-console.png)

### <a name="kudu"></a>Дополнительные инструменты (Kudu)

| | |
|-|-|
| ![Kudu приложения-функции на портале Azure](./media/functions-how-to-use-azure-function-app-settings/function-app-advanced-tools.png) | Дополнительные средства для службы приложений (которые также называются Kudu) предоставляют доступ к расширенным административным функциям для приложения-функции. С помощью Kudu можно управлять системными сведениями, параметрами приложения, переменными среды, заголовками HTTP и переменными сервера. Кроме того, можно также запустить **Kudu**, перейдя на конечную точку SCM для приложения-функции, например `https://<myfunctionapp>.scm.azurewebsites.net/`. |

![Настройка Kudu](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-kudu.png)


### <a name="a-namedeploymentdeployment-options"></a><a name="deployment">Варианты развертывания

| | |
|-|-|
| ![Варианты развертывания приложения-функции на портале Azure](./media/functions-how-to-use-azure-function-app-settings/function-app-deployment-source.png) | Компонент функции позволяет разрабатывать код функций на локальном компьютере. Позднее вы можете передать локальный проект приложения-функции в Azure. Кроме традиционной отправки по FTP, компонент функции позволяет развернуть приложение-функцию с помощью популярных решений непрерывной интеграции, таких как GitHub, Azure DevOps, Dropbox, Bitbucket и др. Дополнительные сведения см. в статье [Непрерывное развертывание для функций Azure](functions-continuous-deployment.md). Чтобы отправить данные вручную с помощью FTP или локального Git, нужно [настроить учетные данные развертывания](functions-continuous-deployment.md#credentials). |


### <a name="cors"></a>CORS

| | |
|-|-|
| ![CORS приложения-функции на портале Azure](./media/functions-how-to-use-azure-function-app-settings/function-app-cors.png) | Чтобы предотвратить выполнение вредоносного кода в службах, служба приложений блокирует вызовы приложений-функций из внешних источников. Компонент функции поддерживает общий доступ к ресурсам независимо от источника (CORS), чтобы вы могли определить "список разрешений" из источников, откуда функции могут принимать удаленные запросы.  |

![Настройка CORS приложения-функции](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-cors.png)

### <a name="auth"></a>Проверка подлинности

| | |
|-|-|
| ![Проверка подлинности приложения-функции на портале Azure](./media/functions-how-to-use-azure-function-app-settings/function-app-authentication.png) | Если функции используют триггер HTTP, можно настроить обязательную предварительную проверку подлинности для вызовов. Служба приложений поддерживает проверку подлинности Azure Active Directory и вход в систему с помощью поставщиков социальных сетей, таких как Facebook, Майкрософт и Twitter. Дополнительные сведения о настройке определенных поставщиков аутентификации см. в разделе [Проверка подлинности и авторизация в службе приложений Azure](../app-service/overview-authentication-authorization.md). |

![Настройка проверки подлинности для приложения-функции](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-authentication.png)


### <a name="swagger"></a>Определение интерфейса API

| | |
|-|-|
| ![Swagger для API приложения-функции на портале Azure](./media/functions-how-to-use-azure-function-app-settings/function-app-api-definition.png) | Компонент функции поддерживает Swagger, что позволяет упростить использование функций, активируемых по HTTP, клиентами. Дополнительные сведения о создании определений API с помощью Swagger см. в статье [Размещение API-интерфейсов RESTful с поддержкой CORS в службе приложений Azure](../app-service/app-service-web-tutorial-rest-api.md). Можно также использовать прокси-серверы для функций, чтобы определить единую поверхность API для нескольких функций. Дополнительные сведения см. в статье [Работа с прокси-серверами для функций Azure](functions-proxies.md). |

![Настройка API приложения-функции](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-apidef.png)



## <a name="next-steps"></a>Дальнейшие действия

+ [Настройка параметров службы приложений Azure](../app-service/configure-common.md)
+ [Непрерывное развертывание для функций Azure](functions-continuous-deployment.md)



