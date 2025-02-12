---
title: включение файла
description: включение файла
services: notification-hubs
author: spelluru
ms.service: notification-hubs
ms.topic: include
ms.date: 02/05/2019
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: fef6122eceda213fb6353ada53033d0d1e27fd7e
ms.sourcegitcommit: 79496a96e8bd064e951004d474f05e26bada6fa0
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/02/2019
ms.locfileid: "67509080"
---
1. Войдите в [консоль Firebase](https://firebase.google.com/console/). Создайте проект Firebase, если его еще нет.
2. После создания проекта выберите **Add Firebase to your Android app** (Добавить Firebase в приложение Android). 

    ![Добавление Firebase в приложение Android](./media/notification-hubs-enable-firebase-cloud-messaging/notification-hubs-add-firebase-to-android-app.png)
3. Выполните следующие действия на странице **Add Firebase to your Android app** (Добавление Firebase в приложение Android): 
    1. Для параметра **Android package name** (Имя пакета Android) скопируйте свое значение **applicationId** в файл приложения build.gradle. В нашем примере он выглядит следующим образом: `com.fabrikam.fcmtutorial1app`. 

        ![Указание имени пакета.](./media/notification-hubs-enable-firebase-cloud-messaging/specify-package-name-fcm-settings.png)
    2. Выберите **Регистрация приложения**. 
4. Выберите **Download google-services.json** (Загрузить google-services.json), сохраните файл в папку **приложения** проекта, а затем выберите **Далее**. 

    ![Загрузка файла google-services.json.](./media/notification-hubs-enable-firebase-cloud-messaging/download-google-service-button.png)
5. Внесите следующие **изменения конфигурации** в проект в Android Studio. 
    1.  В файл build.gradle уровня проекта (&lt;project&gt;/build.gradle) добавьте в раздел **dependencies** следующий текст. 

        ```
        classpath 'com.google.gms:google-services:4.0.1'
        ```
    2. В файл build.gradle уровня приложения (&lt;projectapp&gt;/&lt;-module&gt;/build.gradle) добавьте в раздел **dependencies** следующий текст. 

        ```
        implementation 'com.google.firebase:firebase-core:16.0.1'
        ```

    3. Добавьте следующую строку в конец файла build.gradle уровня приложения после раздела с зависимостями. 

        ```
        apply plugin: 'com.google.gms.google-services'
        ```        
    4. На панели инструментов щелкните **Синхронизировать сейчас**. 
 
        ![Изменения конфигурации build.gradle.](./media/notification-hubs-enable-firebase-cloud-messaging/build-gradle-configurations.png)
6. Щелкните **Далее**. 
7. Выберите **Пропустить этот шаг**. 

    ![Пропуск последнего шага.](./media/notification-hubs-enable-firebase-cloud-messaging/skip-this-step.png)
8. В консоли Firebase щелкните значок шестеренки возле имени проекта. Выберите пункт **Project Settings** (Параметры проекта).

    ![Выбор параметров проекта](./media/notification-hubs-enable-firebase-cloud-messaging/notification-hubs-firebase-console-project-settings.png)
4. Если вы еще не скачали файл google-services.json в папку **app** проекта Android Studio, это можно сделать на этой странице. 
5. Переключитесь на вкладку **Обмен сообщениями в облаке** в верхней части. 
6. Скопируйте и сохраните **Ключ сервера** для последующего использования. Это значение используется для настройки имени центра.
