---
title: Краткое руководство. Синтез речи с помощью службы Unity — службы "Речь"
titleSuffix: Azure Cognitive Services
description: Используйте данное руководство при создании приложения для преобразования речи в текст с помощью Unity и пакета Speech SDK для Unity (бета-версия). После завершения можно будет синтезировать речь из текста в режиме реального времени через микрофон устройства.
services: cognitive-services
author: yinhew
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 6/26/2019
ms.author: yinhew
ms.openlocfilehash: 5240ea45097ce3c0ae7ccbc15a7f99b2f5990832
ms.sourcegitcommit: c63e5031aed4992d5adf45639addcef07c166224
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/28/2019
ms.locfileid: "67467268"
---
# <a name="quickstart-synthesize-speech-with-the-speech-sdk-for-unity-beta"></a>Краткое руководство. Синтез речи с помощью пакета SDK службы "Речь" для Unity (бета-версия)

Также доступны краткие руководства по [распознаванию речи](quickstart-csharp-unity.md).

Используйте данное руководство при создании приложения для преобразования речи в текст с помощью [Unity](https://unity3d.com/) и пакета SDK службы "Речь" для Unity (бета-версия).
После завершения можно будет синтезировать речь из текста в режиме реального времени через микрофон устройства.
Если вы не знакомы с Unity, рекомендуется изучить [Unity User Manual (2018.3)](https://docs.unity3d.com/Manual/UnityManual.html) (Руководство пользователя Unity (2018.3)) перед началом разработки приложения.

> [!NOTE]
> Пакет SDK службы "Речь" для Unity в настоящее время доступен в бета-версии.
> Он поддерживает Windows Desktop (x86 и x64) или универсальную платформу Windows (x86, x64, ARM и ARM64) и Android (x86 ARM32/64).

## <a name="prerequisites"></a>Предварительные требования

Чтобы выполнить этот проект, вам потребуется:

* [Unity 2018.3 или более поздней версии](https://store.unity.com/) с [Unity 2019.1, добавляющий поддержку для UWP ARM64](https://blogs.unity3d.com/2019/04/16/introducing-unity-2019-1/#universal)
* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/)
     * Для поддержки ARM64 установите [дополнительные инструменты сборки для ARM64 и Windows 10 SDK для ARM64](https://blogs.windows.com/buildingapps/2018/11/15/official-support-for-windows-10-on-arm-development/) 
* Ключ подписки для службы "Речь". [Его можно получить бесплатно](get-started.md).

## <a name="create-a-unity-project"></a>Создание проекта Unity

* Запустите Unity и на вкладке **Проекты** выберите **Создать**.
* Для **имени проекта** укажите значение **csharp-unity**, для **шаблона** — **3D** и выберите расположение.
  Затем щелкните **Создать проект**.
* Через некоторое время должно появиться окно Unity Editor.

## <a name="install-the-speech-sdk"></a>Установка пакета SDK службы "Речь"

[!INCLUDE [License Notice](../../../includes/cognitive-services-speech-service-license-notice.md)]

* Пакет SDK службы "Речь" для Unity (бета-версия) упакован как пакет ресурсов Unity (.unitypackage).
  Скачать эту версию можно [здесь](https://aka.ms/csspeech/unitypackage).
* Импортируйте пакет SDK службы "Речь", выбрав **Ресурсы-контейнеры** > **Импорт пакета** > **Custom Package** (Пользовательский пакет).
  Дополнительные сведения см. в [документации по Unity](https://docs.unity3d.com/Manual/AssetPackages.html).
* В средстве выбора файлов выберите файл пакета SDK службы "Речь" формата UNITYPACKAGE, скачанный ранее.
* Убедитесь, что выбраны все файлы, а затем щелкните **Импорт**:

  ![Снимок экрана с Unity Editor при импортировании пакета ресурсов SDK службы "Речь" для Unity](media/sdk/qs-csharp-unity-01-import.png)

## <a name="add-ui"></a>Добавление пользовательского интерфейса

В нашу сцену мы добавим минимальный пользовательский интерфейс, состоящий из поля ввода текста для синтеза речи, кнопки активации синтеза речи и текстового поля для отображения результата.

* В [окне иерархии](https://docs.unity3d.com/Manual/Hierarchy.html) (по умолчанию слева) показан пример сцены создания проекта Unity.
* В верхней части окна иерархии щелкните **Create** (Создать), а затем выберите **UI** (Пользовательский интерфейс) > **Input Field** (Поле ввода).
* Будут созданы три игровых объекта, которые можно увидеть в окне иерархии: объект **Input Field** (Поле ввода), вложенный в объект **Canvas** (Полотно) и объект **EventSystem**.
* [Перемещайтесь по представлению сцены](https://docs.unity3d.com/Manual/SceneViewNavigation.html) для удобного просмотра полотна и поля ввода в [представлении сцены](https://docs.unity3d.com/Manual/UsingTheSceneView.html).
* Щелкните объект **Input Field** (Поле ввода) в окне иерархии, чтобы отобразить его параметры в [окне инспектора](https://docs.unity3d.com/Manual/UsingTheInspector.html) (по умолчанию справа).
* Задайте для свойств **Pos X** и **Pos Y** значение **0**, чтобы поле ввода было расположено в середине полотна.
* Снова нажмите кнопку **Create** (Создать) в верхней части окна иерархии и выберите **UI** (Пользовательский интерфейс) > **Button** (Кнопка), чтобы создать кнопку.
* Щелкните объект **Кнопка** в окне иерархии, чтобы отобразить его параметры в [окне инспектора](https://docs.unity3d.com/Manual/UsingTheInspector.html) (по умолчанию справа).
* Задайте для свойств **Pos X** и **Pos Y** значения **0** и **-48**. Задайте для свойств **Width** (Ширина) и **Height** (Высота) значения **160** и **30**, чтобы убедиться, что кнопка и поле ввода не перекрываются.
* Снова нажмите кнопку **Создать** в верхней части окна иерархии и выберите **Пользовательский интерфейс** > **Текст**, чтобы создать текстовое поле.
* Щелкните объект **Текст** в окне иерархии, чтобы отобразить его параметры в [окне инспектора](https://docs.unity3d.com/Manual/UsingTheInspector.html) (по умолчанию справа).
* Задайте для свойств **Pos X** и **Pos Y** значения **0** и **80**. Задайте для свойств **Width** (Ширина) и **Height** (Высота) значения **320** и **80**, чтобы убедиться, что текстовое поле и кнопка не перекрываются.
* Нажмите кнопку **Create** (Создать) в верхней части окна иерархии еще раз и выберите **Audio** (Аудио) > **Audio Source** (Аудиоисточник) для создания источника аудио.

По завершении пользовательский интерфейс должен выглядеть примерно так:

[![Снимок экрана пользовательского интерфейса краткого руководства в Unity Editor](media/sdk/qs-tts-csharp-unity-ui-inline.png)](media/sdk/qs-tts-csharp-unity-ui-expanded.png#lightbox)

## <a name="add-the-sample-code"></a>Добавление примеров кода

1. В [окне проекта](https://docs.unity3d.com/Manual/ProjectView.html) (по умолчанию слева внизу) нажмите кнопку **Создать**, а затем выберите **Скрипт C#** . Назовите скрипт `HelloWorld`.

1. Отредактируйте скрипт, дважды щелкнув его.

   > [!NOTE]
   > Чтобы настроить, какой редактор кода будет запускаться, в разделе **Правка** > **Настройки** ознакомьтесь с [руководством пользователя Unity](https://docs.unity3d.com/Manual/Preferences.html).

1. Замените весь код на приведенный ниже:

   [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/text-to-speech/csharp-unity/Assets/Scripts/HelloWorld.cs#code)]

1. Найдите строку `YourSubscriptionKey` и замените ее ключом подписки службы "Речь".

1. Найдите и замените строку `YourServiceRegion` на [регион](regions.md), связанный с вашей подпиской. Например, если вы используете бесплатную пробную версию, ваш регион — `westus`.

1. Сохраните изменения в скрипте.

1. Вернитесь в Unity Editor и добавьте скрипт в качестве компонента одного из игровых объектов.

   * Щелкните объект **Полотно** в окне иерархии. Откроются параметры в [окне инспектора](https://docs.unity3d.com/Manual/UsingTheInspector.html) (по умолчанию справа).
   * Нажмите кнопку **Добавить компонент** в окне инспектора, затем найдите скрипт HelloWorld, который был создан выше, и добавьте его.
   * Обратите внимание, что компонент Hello World имеет четыре неинициализированные свойства: **Output Text** (Выходной текст), **Input Field** (Поле ввода), **Speak Button** (Кнопка "Произнести") и **Audio Source** (Источник аудио), соответствующие общедоступным свойствам класса `HelloWorld`.
     Чтобы подключить их, щелкните "Выбор объектов" (маленький круглый значок справа от свойства) и выберите объекты текста и кнопки, созданные ранее.

     > [!NOTE]
     > Поле ввода и кнопка также имеют вложенный текстовый объект. Убедитесь, что вы случайно не выбрали его для вывода текста (или переименуйте текстовые объекты, используя поле Name (Имя) в окне инспектора, чтобы избежать этой путаницы).

## <a name="run-the-application-in-the-unity-editor"></a>Запуск приложения в Unity Editor

* Нажмите кнопку **Воспроизвести** на панели инструментов Unity Editor (под строкой меню).

* После запуска приложения введите текст в поле ввода и нажмите кнопку. Введенный текст передается в службы "Речь" и синтезируется в речь, которая воспроизводится через динамик.

  [![Снимок экрана запущенного краткого руководства в окне Unity Game](media/sdk/qs-tts-csharp-unity-output-inline.png)](media/sdk/qs-tts-csharp-unity-output-expanded.png#lightbox)

* Проверьте [окно консоли](https://docs.unity3d.com/Manual/Console.html) на наличие сообщений отладки.

* По завершении синтеза речи нажмите кнопку **Play** (Воспроизвести) на панели инструментов Unity Editor, чтобы остановить работу приложения.

## <a name="additional-options-to-run-this-application"></a>Дополнительные параметры для запуска этого приложения

Это приложение также можно развернуть на Android как автономное приложение Windows или приложение UWP.
Ознакомьтесь с [примером репозитория](https://aka.ms/csspeech/samples) в папке quickstart/csharp-unity, который описывает конфигурацию этих дополнительных целей.

## <a name="next-steps"></a>Дополнительная информация

> [!div class="nextstepaction"]
> [Примеры для C# на сайте GitHub](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>См. также

- [Настройка голосов](how-to-customize-voice-font.md)
- [Запись образцов голосов](record-custom-voice-samples.md)
