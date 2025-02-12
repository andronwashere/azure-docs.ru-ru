---
title: Импорт или экспорт данных с помощью конфигурации приложения Azure | Документация Майкрософт
description: Узнайте, как для импорта или экспорта данных в или из конфигурации приложения Azure
services: azure-app-configuration
documentationcenter: ''
author: yegu-ms
manager: balans
editor: ''
ms.assetid: ''
ms.service: azure-app-configuration
ms.topic: conceptual
ms.date: 02/24/2019
ms.author: yegu
ms.custom: mvc
ms.openlocfilehash: 377c5088d39821e87412c517540b3190b0a14a00
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "66393286"
---
# <a name="import-or-export-configuration-data"></a>Импорт и экспорт данных конфигурации

Azure поддерживают данные конфигурации приложения операции импорта и экспорта. Использовать эти операции для работы с данными конфигурации в массового и обмена данными между хранилища конфигурации приложения и код проекта. Например можно настроить хранилище конфигураций одно приложение для тестирования, а другой для рабочей среды. Затем можно скопировать параметры приложения между ними посредством файла, таким образом, чтобы не нужно вводить данные в два раза.

В этой статье рекомендации по импорту и экспорту данных с конфигурацией приложения.

## <a name="import-data"></a>Импорт данных

Импорт привносит конфигурации хранилища данных в конфигурацию приложения из существующего источника, а не вручную, указав его. Используйте функцию импорта для переноса данных в хранилище конфигурации приложения или к объединенным данным из нескольких источников. Конфигурация приложения поддерживает импорт из файла JSON, YAML или свойства.

Импорт данных с помощью [портала Azure](https://portal.azure.com) или [Azure CLI](./scripts/cli-import.md). На портале Azure сделайте следующее:

1. Перейдите в хранилище конфигурации приложения и выберите **импорта и экспорта**.

2. На **импорта** выберите **источника службы** > **файл конфигурации**.

3. Выберите **для языка** > **тип файла**.

4. Выберите **папку** значок и перейдите к файлу для импорта.

    ![Импорт файла](./media/import-file.png)

5. Выберите **разделителя**и при необходимости введите **префикса** для импортированных имена ключей.

6. При необходимости выберите **метка**.

7. Выберите **применить** завершить импорт.

    ![Импорт файла завершена](./media/import-file-complete.png)

## <a name="export-data"></a>Экспорт данных

Экспорт записывает данные конфигурации, хранимые в конфигурации приложения в другое место назначения. Функция экспорта, например, для сохранения данных в хранилище конфигурации приложения в файл, внедренный в код приложения во время развертывания.

Экспорт данных с помощью [портала Azure](https://portal.azure.com) или [Azure CLI](./scripts/cli-export.md). На портале Azure сделайте следующее:

1. Перейдите в хранилище конфигурации приложения и выберите **импорта и экспорта**.

2. На **Экспорт** выберите **целевая служба** > **файл конфигурации**.

3. При необходимости введите **префикса** и выберите **метка** и в определенный момент времени для ключей, для экспорта.

4. Выберите **тип файла** > **разделителя**.

5. Выберите **применить** чтобы завершить экспорт.

    ![Экспорт файла завершена](./media/export-file-complete.png)

## <a name="next-steps"></a>Дальнейшие действия

> [!div class="nextstepaction"]
> [Создание веб-приложения ASP.NET Core](./quickstart-aspnet-core-app.md)  
