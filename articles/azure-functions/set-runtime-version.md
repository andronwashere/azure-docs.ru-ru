---
title: Выбор целевых версий среды выполнения Функций Azure
description: Решение "Функции Azure" поддерживает разные версии среды выполнения. Узнайте, как указать версию среды выполнения приложения-функции, размещенного в Azure.
services: functions
author: ggailey777
manager: jeconnoc
ms.service: azure-functions
ms.topic: conceptual
ms.date: 11/26/2018
ms.author: glenga
ms.openlocfilehash: 2047f11272c6154d4443e889eff24401c2f73afa
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "67067693"
---
# <a name="how-to-target-azure-functions-runtime-versions"></a>Выбор целевых версий среды выполнения Функций Azure

Приложение-функция работает в определенной версии среды выполнения для решения "Функции Azure". Есть два основных номера версии: [1.x и 2.x](functions-versions.md). По умолчанию приложения-функции, созданные в версии 2.x среды выполнения. Из этой статьи вы узнаете, как настроить приложение-функцию Azure для выполнения в выбранной версии. Сведения о настройке локальной среды разработки для определенной версии см. в разделе [Как программировать и тестировать функции Azure в локальной среде](functions-run-local.md).

## <a name="automatic-and-manual-version-updates"></a>Обновление версий автоматически и вручную

Функции Azure вы можете выбрать определенную версию среды выполнения с помощью `FUNCTIONS_EXTENSION_VERSION` параметр приложения в приложении-функции. Приложение-функция будет работать в указанной основной версии, пока вы явно не переместите его в новую.

Если указать только основной номер версии ("~2" для 2.x или "~1" для 1.x), приложение-функция автоматически обновляется до новых вспомогательных версий среды выполнения по мере их выпуска. Во вспомогательные версии не вносятся критически важные изменения. При указании дополнительного номера версии (например, "2.0.12345") приложение-функция будет закреплено за этой версией, пока вы явно не укажете другую.

> [!NOTE]
> Если закрепить для определенной версии функции Azure и затем попытайтесь опубликовать в Azure с помощью Visual Studio, появится диалоговое окно появится запрос на обновление до последней версии или Отмена публикации. Чтобы избежать этого, добавьте `<DisableFunctionExtensionVersionUpdate>true</DisableFunctionExtensionVersionUpdate>` свойство в вашей `.csproj` файл.

Если новая версия общедоступна, выполнить обновление до нее можно при помощи запроса на портале. Перейдя на новую версию, вы всегда сможете вернуться к предыдущей с помощью параметра приложения `FUNCTIONS_EXTENSION_VERSION`.

Изменение версии среды выполнения сопровождается перезапуском приложения-функции.

Вы можете включить автоматическое обновление, задав для параметра приложения `FUNCTIONS_EXTENSION_VERSION` соответствующее значение (сейчас доступны значения "~1" для среды выполнения версии 1.x и "~2" для версии 2.x).

## <a name="view-and-update-the-current-runtime-version"></a>Просмотр и обновление текущей версии среды выполнения

Можно изменить версию среды выполнения, используемый приложением-функцией. Так как возможно внесение критических изменений, следует изменять версию среды выполнения только перед созданием любых функций в приложении-функции. Версию среды выполнения определяет параметр `FUNCTIONS_EXTENSION_VERSION`, но изменять это значение напрямую не следует. Вместо этого воспользуйтесь порталом Azure. Это обусловлено тем, что на портале изменения проверяются и при необходимости вносятся другие связанные с ними изменения.

### <a name="from-the-azure-portal"></a>на портале Azure;

[!INCLUDE [Set the runtime version in the portal](../../includes/functions-view-update-version-portal.md)]

> [!NOTE]
> С помощью портала Azure, нельзя изменить версию среды выполнения для приложения-функции, которая уже содержит функции.

### <a name="view-and-update-the-runtime-version-using-azure-cli"></a>Использование Azure CLI

Параметр `FUNCTIONS_EXTENSION_VERSION` также можно просмотреть и задать с помощью интерфейса командной строки Azure.

>[!NOTE]
>Поскольку версия среды выполнения может влиять на другие параметры, следует изменить версию на портале. При изменении версии среды выполнения портал автоматически сделает другие необходимые обновления, например версии Node.js и стека среды выполнения.  

В Azure CLI просмотрите текущую версию среды выполнения с помощью команды [az functionapp config appsettings set](/cli/azure/functionapp/config/appsettings).

```azurecli-interactive
az functionapp config appsettings list --name <function_app> \
--resource-group <my_resource_group>
```

В этом коде следует заменить `<function_app>` именем приложения-функции. Также замените `<my_resource_group>` именем группы ресурсов для приложения-функции. 

В следующих выходных данных (сокращенных для ясности) есть параметр `FUNCTIONS_EXTENSION_VERSION`:

```output
[
  {
    "name": "FUNCTIONS_EXTENSION_VERSION",
    "slotSetting": false,
    "value": "~2"
  },
  {
    "name": "FUNCTIONS_WORKER_RUNTIME",
    "slotSetting": false,
    "value": "dotnet"
  },
  
  ...
  
  {
    "name": "WEBSITE_NODE_DEFAULT_VERSION",
    "slotSetting": false,
    "value": "8.11.1"
  }
]
```

Обновить параметр `FUNCTIONS_EXTENSION_VERSION` в приложении-функции можно с помощью команды [az functionapp config appsettings set](/cli/azure/functionapp/config/appsettings).

```azurecli-interactive
az functionapp config appsettings set --name <function_app> \
--resource-group <my_resource_group> \
--settings FUNCTIONS_EXTENSION_VERSION=<version>
```

Замените `<function_app>` на имя приложения-функции. Также замените `<my_resource_group>` именем группы ресурсов для приложения-функции. Замените также `<version>` на значение допустимой версии среды выполнения (для версии 1.x) или на значение `~2` (для версии 2).

Эту команду можно выполнить в [Azure Cloud Shell](../cloud-shell/overview.md), выбрав **Попробовать** в предыдущем примере кода. Также можно использовать [Azure CLI локально](/cli/azure/install-azure-cli) для выполнения этой команды после выполнения команды [az login](/cli/azure/reference-index#az-login) для входа.



## <a name="next-steps"></a>Дальнейшие действия

> [!div class="nextstepaction"]
> [Выбор среды выполнения 2.0 в локальной среде разработки](functions-run-local.md)

> [!div class="nextstepaction"]
> [См. заметки о выпуске для версий среды выполнения](https://github.com/Azure/azure-webjobs-sdk-script/releases)
