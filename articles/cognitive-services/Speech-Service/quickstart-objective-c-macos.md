---
title: Краткое руководство. Распознавание речи с помощью Objective-C в службе "Речь"
titleSuffix: Azure Cognitive Services
description: Узнайте, как распознавать речь в macOS с помощью Objective-C и пакета SDK службы "Речь".
services: cognitive-services
author: chlandsi
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 07/05/2019
ms.author: chlandsi
ms.openlocfilehash: 25f341d167cecd765fd89d9286708d0bd8df6dd2
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2019
ms.locfileid: "67603033"
---
# <a name="quickstart-recognize-speech-in-objective-c-on-macos-using-the-speech-sdk"></a>Краткое руководство. Распознавание речи в macOS с помощью Objective-C и пакета SDK службы "Речь"

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

Из этой статьи вы узнаете, как с помощью пакета SDK для службы "Речь" в Cognitive Services можно создать приложение macOS на Objective-C, которое будет преобразовывать микрофонную запись речи в текст.

## <a name="prerequisites"></a>Предварительные требования

Предварительные требования для начала работы:

* [Ключ подписки](get-started.md) для службы "Речь".
* Компьютер под управлением macOS 10.13 или последующей версии с [Xcode 9.4.1](https://geo.itunes.apple.com/us/app/xcode/id497799835?mt=12) или последующей версии.

## <a name="get-the-speech-sdk-for-macos"></a>Получение пакета SDK службы "Речь" для macOS

[!INCLUDE [License Notice](../../../includes/cognitive-services-speech-service-license-notice.md)]

Текущая версия пакета SDK для распознавания речи для Cognitive Services — `1.6.0`.

Пакет SDK службы "Речь" в Cognitive Services для Mac сейчас доступен в виде пакета платформы.
Его можно использовать в проектах Xcode в виде диспетчера зависимостей [CocoaPods](https://cocoapods.org/) или скачать по адресу https://aka.ms/csspeech/macosbinary и выполнить привязку вручную. В этом руководстве используется диспетчер зависимостей CocoaPods.

## <a name="create-an-xcode-project"></a>Создание проекта Xcode

Запустите Xcode и создайте новый проект, щелкнув **Файл** > **Новый** > **Проект**.
В диалоговом окне выбора шаблона выберите шаблон Cocoa App.

В следующих диалоговых окнах задайте следующие параметры:

1. Диалоговое окно Project Options (Параметры проекта)
    1. Введите имя приложения быстрого запуска, например `helloworld`.
    1. Если у вас есть учетная запись разработчика Apple, введите соответствующие название и идентификатор организации. В целях тестирования вы можете выбрать любое имя, например `testorg`. Чтобы подписать приложение, вам нужен соответствующий профиль подготовки. Дополнительные сведения см. на [сайте разработчиков Apple](https://developer.apple.com/).
    1. Убедитесь в том, что для проекта выбран язык Objective-C.
    1. Снимите флажки использования раскадровок и создания приложения на основе документа. С помощью программных средств создастся простой пользовательский интерфейс для примера приложения.
    1. Снимите все флажки тестов и основных данных.
    ![Параметры проекта](media/sdk/qs-objectivec-macos-project-settings.png)
1. Выбор каталога проекта
    1. Выберите каталог для размещения проекта. Это создаст в вашем домашнем каталоге каталог `helloworld`, содержащий все файлы проекта Xcode.
    1. Для этого примера проекта отключите создание репозитория Git.
1. Задайте права доступа к сети и микрофону. Щелкните имя приложения в первой строке в области обзора слева, чтобы открыть настройки приложения, и перейдите на вкладку "Capabilities" (Возможности).
    1. Включите для приложения параметр "App sandbox" (Песочница приложения).
    1. Установите флажки "Outgoing Connections" (Исходящие подключения) и "Microphone" (Микрофон) для доступа к микрофону.
    ![Параметры "песочницы"](media/sdk/qs-objectivec-macos-sandbox.png)
1. Для приложения также необходимо объявить использование микрофона в файле `Info.plist`. В области обзора щелкните имя файла и добавьте ключ "Privacy — Microphone Usage Description" (Конфиденциальность — описание использования микрофона) со значением "Microphone is needed for speech recognition" (Микрофон необходим для распознавания речи).
    ![Параметры в файле Info.plist](media/sdk/qs-objectivec-macos-info-plist.png)
1. Закройте проект Xcode. Вы воспользуетесь другим его экземпляром позже после настройки CocoaPods.

## <a name="install-the-sdk-as-a-cocoapod"></a>Установка пакета SDK в качестве диспетчера зависимостей CocoaPods

1. Установите диспетчер зависимостей CocoaPods, следуя его [инструкциям по установке](https://guides.cocoapods.org/using/getting-started.html).
1. Перейдите в каталог с примером приложения (`helloworld`). Поместите текстовый файл с именем `Podfile` и следующим содержимым в этот каталог: [!code-objectivec[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/objectivec-macos/helloworld/Podfile)]
1. В терминале перейдите к каталогу `helloworld` и выполните команду `pod install`. При этом будет создана рабочая область Xcode `helloworld.xcworkspace`, содержащая пример приложения и пакет SDK для службы "Речь" в виде зависимости. Эта рабочая область понадобится позже.

## <a name="add-the-sample-code"></a>Добавление примеров кода

1. Откройте рабочую область `helloworld.xcworkspace` в Xcode.
1. Замените содержимое автоматически сгенерированного файла `AppDelegate.m` на:  
   [!code-objectivec[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/objectivec-macos/helloworld/helloworld/AppDelegate.m#code)]
1. Замените строку `YourSubscriptionKey` своим ключом подписки.
1. Замените строку `YourServiceRegion` значением [региона](regions.md), связанного с подпиской (например, `westus` для бесплатной пробной подписки).

## <a name="build-and-run-the-sample"></a>Сборка и запуск примера

1. Сделайте вывод отладки видимым: **View**(Представление) > **Debug Area**(Отладка области) > **Activate Console**(Активировать консоль).
1. Скомпилируйте и запустите пример кода, выбрав в меню **Product** > **Run** (Продукт -> Запустить) или нажав кнопку **Play** (Воспроизвести).
1. Если нажать кнопку и произнести несколько слов, эти слова должны быть распознаны и отображены в нижней части экрана. При первом запуске приложения должен отобразится запрос о предоставлении приложению доступа к микрофону компьютера.

## <a name="next-steps"></a>Дополнительная информация

> [!div class="nextstepaction"]
> [Ознакомьтесь с примерами на Objective-C на сайте GitHub](https://aka.ms/csspeech/samples)
