---
title: Руководство по Обнаружение и выделение лиц на изображении с помощью пакета SDK для Android
titleSuffix: Azure Cognitive Services
description: С помощью этого руководства вы создадите простое приложение Android, которое использует API распознавания лиц для определения и выделения лиц на изображении.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: tutorial
ms.date: 07/03/2019
ms.author: pafarley
ms.openlocfilehash: 366c0c50cee521c5e70496403fd77211a875065f
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2019
ms.locfileid: "67606761"
---
# <a name="tutorial-create-an-android-app-to-detect-and-frame-faces-in-an-image"></a>Руководство по Создание приложения Android для обнаружения и выделения лиц на изображении

С помощью этого руководства вы создадите простое приложение для Android, которое использует API распознавания лиц Azure, поставляемый через пакет SDK для Java, для определения лиц на изображении. Приложение отображает выбранное изображение и рисует рамку вокруг каждого распознанного лица.

В этом учебнике описаны следующие процедуры.

> [!div class="checklist"]
> - Создание приложения Android
> - установка клиентской библиотеки API распознавания лиц;
> - использование клиентской библиотеки для обнаружения лиц на изображении;
> - выделение рамкой каждого обнаруженного лица;

![Снимок экрана устройства Android с фотографией, на которой лица выделены красными прямоугольниками](../Images/android_getstarted2.1.PNG)

Полный пример кода доступен в репозитории [Cognitive Services Face Android](https://github.com/Azure-Samples/cognitive-services-face-android-sample) на сайте GitHub.

Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/), прежде чем начинать работу. 

## <a name="prerequisites"></a>Предварительные требования

- Ключ подписки на API распознавания лиц. Вы можете получить ключ бесплатной пробной подписки на странице [Пробная версия Cognitive Services](https://azure.microsoft.com/try/cognitive-services/?api=face-api). Или следуйте инструкциям в руководстве по [созданию учетной записи Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account), чтобы получить подписку на API распознавания лиц и свой ключ.
- Интерфейс [Android Studio](https://developer.android.com/studio/) с уровнем API не ниже 22 (требуется для клиентской библиотеки службы распознавания лиц).

## <a name="create-the-android-studio-project"></a>Создание проекта Android Studio

Чтобы создать проект нового приложения Android, сделайте следующее.

1. В Android Studio щелкните **Start a new Android Studio project** (Создать проект Android Studio).
1. На экране **Create Android Project** (Создание проекта Android) при необходимости измените значения в полях по умолчанию, а затем нажмите кнопку **Next** (Далее).
1. На экране **Target Android Devices** (Целевые устройства Android) используйте селектор раскрывающегося списка, чтобы выбрать **API 22** или более поздней версии, а затем нажмите кнопку **Next** (Далее).
1. Выберите значение **Empty Activity** (Пустое действие), затем нажмите кнопку **Next** (Далее).
1. Снимите флажок **Backwards Compatibility** (Обратная совместимость), затем нажмите кнопку **Finish** (Готово).

## <a name="add-the-initial-code"></a>Добавление исходного кода

### <a name="create-the-ui"></a>Создание пользовательского интерфейса

Откройте файл *activity_main.xml*. В редакторе макетов выберите вкладку **Text** (Текст) и замените ее содержимое следующим кодом.

[!code-xml[](~/cognitive-services-face-android-detect/FaceTutorial/app/src/main/res/layout/activity_main.xml?range=1-18)]

### <a name="create-the-main-class"></a>Создание главного класса

Откройте файл *MainActivity.java* и замените имеющиеся инструкции `import` в этом окне следующим кодом.

[!code-java[](~/cognitive-services-face-android-detect/FaceTutorial/app/src/main/java/com/contoso/facetutorial/MainActivity.java?range=3-11)]

Замените содержимое файла класса **MainActivity** следующим кодом: Создается обработчик событий для элемента **Button**, который запускает новое действие, позволяя пользователю выбрать изображение. Изображение отображается в **ImageView**.

[!code-java[](~/cognitive-services-face-android-detect/FaceTutorial/app/src/main/java/com/contoso/facetutorial/MainActivity.java?range=29-68)]

### <a name="try-the-app"></a>Использование приложения

Закомментируйте вызов **detectAndFrame** в методе **onActivityResult**. В меню выберите команду **Запустить**, чтобы протестировать приложение. Когда приложение откроется в эмуляторе или подключенном устройстве, нажмите кнопку **Обзор** внизу. Появится диалоговое окно выбора файла на устройстве. Выберите изображение и убедитесь, что оно отображается в окне. Закройте приложение и перейдите к следующему шагу.

![Снимок экрана устройства Android с фотографией лиц](../Images/android_getstarted1.1.PNG)

## <a name="add-the-face-sdk"></a>Добавление пакета SDK распознавания лиц

### <a name="add-the-gradle-dependency"></a>Добавление зависимости Gradle

В области **Project** (Проект) с помощью селектора раскрывающегося списка выберите **Android**. Разверните раздел **Gradle Scripts** (Скрипты Gradle), а затем откройте *build.gradle (Module: app)* . Добавьте зависимость `com.microsoft.projectoxford:face:1.4.3` для клиентской библиотеки службы распознавания лиц, как показано на следующем снимке экрана, а затем щелкните **Sync Now** (Синхронизировать).

![Снимок экрана Android Studio с изображением файла приложения build.gradle](../Images/face-tut-java-gradle.png)

### <a name="add-the-face-related-project-code"></a>Добавление кода проекта, связанного с распознаванием лиц

В файл **MainActivity.java** добавьте следующие операторы `import`.

[!code-java[](~/cognitive-services-face-android-detect/FaceTutorial/app/src/main/java/com/contoso/facetutorial/MainActivity.java?range=13-14)]

Затем вставьте следующий код в класс **MainActivity** над методом **onCreate**:

[!code-java[](~/cognitive-services-face-android-detect/FaceTutorial/app/src/main/java/com/contoso/facetutorial/MainActivity.java?range=17-27)]

Замените `<Subscription Key>` ключом своей подписки. Кроме того, замените `<API endpoint>` конечной точкой API распознавания лиц, используя соответствующий идентификатор региона для ключа (список конечных точек для всех регионов см. в [документации по API распознавания лиц](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)). Ключи бесплатной пробной подписки создаются в регионе **westus**.

В области **Project** (Проект) разверните раздел **app** (приложение), затем **manifests** (манифесты) и откройте файл *AndroidManifest.xml*. Добавьте следующий элемент в качестве непосредственного дочернего элемента для элемента `manifest`:

[!code-xml[](~/cognitive-services-face-android-detect/FaceTutorial/app/src/main/AndroidManifest.xml?range=5)]

## <a name="upload-image-and-detect-faces"></a>Передача изображений и определение лиц

Ваше приложение будет распознавать лица, вызывая метод **faceClient.Face.DetectWithStreamAsync**, который инкапсулирует REST API [обнаружения](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) и возвращает список экземпляров элементов **Face**.

Каждый возвращенный элемент **Face** выделяется прямоугольником, указывающим его расположение, и дополняется рядом необязательных атрибутов лица. В этом примере запрашиваются только прямоугольники для лиц.

Вставьте следующие два метода в класс **MainActivity**. Обратите внимание, что после завершения определения лиц приложение вызывает метод **drawFaceRectanglesOnBitmap** для изменения **ImageView**. Вы определите этот метод далее.

[!code-java[](~/cognitive-services-face-android-detect/FaceTutorial/app/src/main/java/com/contoso/facetutorial/MainActivity.java?range=70-150)]

## <a name="draw-face-rectangles"></a>Рисование прямоугольников для лиц

Вставьте следующий вспомогательный метод в класс **MainActivity**. Этот метод рисует прямоугольник вокруг каждого распознанного лица, используя координаты прямоугольника каждого экземпляра **Face**.

[!code-java[](~/cognitive-services-face-android-detect/FaceTutorial/app/src/main/java/com/contoso/facetutorial/MainActivity.java?range=152-173)]

Наконец, раскомментируйте вызов метода **detectAndFrame** в методе **onActivityResult**.

## <a name="run-the-app"></a>Запуск приложения

Запустите приложение и с помощью обзора выберите изображение, на котором присутствует лицо. Подождите несколько секунд, чтобы служба распознавания лиц смогла ответить. На каждом лице на изображении отобразится красный прямоугольник.

![Снимок экрана Android с красными прямоугольниками, нарисованными вокруг лиц](../Images/android_getstarted2.1.PNG)

## <a name="next-steps"></a>Дополнительная информация

С помощью этого руководства вы изучили базовый процесс использования пакета SDK Java для API распознавания лиц и создали приложение, в котором лица на изображениях отображаются и выделяются рамкой. Ознакомьтесь с дополнительными сведениями об определении лиц.

> [!div class="nextstepaction"]
> [Как обнаруживать лица на изображении](../Face-API-How-to-Topics/HowtoDetectFacesinImage.md)
