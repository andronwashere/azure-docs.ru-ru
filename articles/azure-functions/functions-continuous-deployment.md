---
title: Непрерывное развертывание для Функций Azure | Документация Майкрософт
description: Публикация функций, используйте функции непрерывного развертывания службы приложений Azure.
services: functions
documentationcenter: na
author: ggailey777
manager: jeconnoc
ms.assetid: 361daf37-598c-4703-8d78-c77dbef91643
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 09/25/2016
ms.author: glenga
ms.openlocfilehash: dd1605aa2f5fc9e0b4bc790bae2a1c20cb3ea048
ms.sourcegitcommit: ccb9a7b7da48473362266f20950af190ae88c09b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2019
ms.locfileid: "67594535"
---
# <a name="continuous-deployment-for-azure-functions"></a>Непрерывное развертывание для Функций Azure

Функции Azure позволяют непрерывно развертывает код с помощью [интеграция системы управления версиями](functions-deployment-technologies.md#source-control). Интеграция системы управления версиями обеспечивает рабочий процесс, в котором обновление кода активировать развертывание в Azure. Если вы работали с функциями Azure, начать обучение с просмотра [Обзор функций Azure](functions-overview.md).

Непрерывное развертывание оно подходит для проектов, где интегрировать несколько и часто вклад. При использовании непрерывного развертывания, вы обеспечиваете единый источник истины для вашего кода, который позволяет группам сотрудничать. Непрерывное развертывание можно настроить в функциях Azure из следующих расположений источника кода:

* [Azure репозиториев](https://azure.microsoft.com/services/devops/repos/)
* [GitHub](https://github.com)
* [Bitbucket;](https://bitbucket.org/)

Единицей развертывания для функций в Azure является приложение-функция. Все функции в приложении-функции, развертываются в то же время. После включения непрерывного развертывания, доступ к коду функции на портале Azure работает в режиме *только для чтения* так, как источник истины установлен в другом месте.

## <a name="requirements-for-continuous-deployment"></a>Требования для непрерывного развертывания

Для непрерывного развертывания для успешной структуре каталогов должен быть совместим с структуру основных папок, которая ожидает "функции Azure".

[!INCLUDE [functions-folder-structure](../../includes/functions-folder-structure.md)]

## <a name="credentials"></a>Настройка непрерывного развертывания

Чтобы настроить непрерывное развертывание для имеющегося приложения-функции, выполните следующие действия. Здесь показана интеграция с репозиторием GitHub, но аналогичная процедура подходит для репозиториев Azure или другие репозитории исходного кода.

1. В приложении-функции в [портала Azure](https://portal.azure.com)выберите **функции платформы** > **центр развертывания**.

    ![Откройте центр развертывания](./media/functions-continuous-deployment/platform-features.png)

2. В **центр развертывания**выберите **GitHub**, а затем выберите **Authorize**. Если вы уже задали GitHub, выберите **Продолжить**. 

    ![Центр развертывания службы приложений Azure](./media/functions-continuous-deployment/github.png)

3. В GitHub, выберите **авторизовать AzureAppService** кнопки. 

    ![Авторизация службы приложений Azure](./media/functions-continuous-deployment/authorize.png)
    
    В **центр развертывания** на портале Azure выберите **Продолжить**.

4. Выберите один из следующих поставщиков:

    * **Служба сборок службы приложений**: Рекомендации, когда нет необходимости сборки или при необходимости универсальной сборки.
    * **Конвейеры Azure (Предварительная версия)** : Лучше, когда требуется дополнительный контроль над сборки. Этот поставщик в настоящее время доступна Предварительная версия.

    ![Выберите поставщик построения](./media/functions-continuous-deployment/build.png)

5. Настройте сведения, относящиеся к указанный параметр управления источника. Для GitHub, необходимо ввести или выбрать значения для **организации**, **репозитория**, и **ветви**. Значения основаны на расположение вашего кода. Затем выберите **Продолжить**.

    ![Настройка GitHub](./media/functions-continuous-deployment/github-specifics.png)

6. Просмотрите все сведения, а затем выберите **Готово** для завершения конфигурации развертывания.

    ![Сводка](./media/functions-continuous-deployment/summary.png)

После завершения процесса в приложение развертывается всего кода из указанного источника. После этого изменения в источнике развертывания активировать развертывание этих изменений для приложения-функции в Azure.

## <a name="deployment-scenarios"></a>Сценарии развертывания

<a name="existing"></a>

### <a name="move-existing-functions-to-continuous-deployment"></a>Перемещение имеющихся функций в среду непрерывного развертывания

Если вы уже написали функций [портала Azure](https://portal.azure.com) и вы хотите скачать содержимое приложения перед переключением в среду непрерывного развертывания, перейдите к **Обзор** вкладке приложения-функции. Выберите **скачать содержимое приложения** кнопки.

![Скачать содержимое приложения](./media/functions-continuous-deployment/download.png)

> [!NOTE]
> После настройки непрерывной интеграции, больше не может изменить исходные файлы на портале функций.

## <a name="next-steps"></a>Следующие шаги

> [!div class="nextstepaction"]
> [Рекомендации по функциям Azure](functions-best-practices.md)
