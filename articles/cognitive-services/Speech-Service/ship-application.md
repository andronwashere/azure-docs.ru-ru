---
title: Разработка приложений с помощью речевого пакета SDK — служба речи
titleSuffix: Azure Cognitive Services
description: Узнайте, как создавать приложения с помощью пакета SDK службы "Речь".
services: cognitive-services
author: jhakulin
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 07/23/2019
ms.author: jhakulin
ms.custom: seodec18
ms.openlocfilehash: 8192a2d731104fe898ca128a1d989783698567da
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/26/2019
ms.locfileid: "68559160"
---
# <a name="ship-an-application"></a>Доставка приложения

При распространении пакета SDK для службы "Речь" в Azure Cognitive Services соблюдайте требования [лицензии на пакет SDK для службы "Речь"](https://aka.ms/csspeech/license201809), а также [уведомлений о стороннем ПО](https://csspeechstorage.blob.core.windows.net/drop/1.0.0/ThirdPartyNotices.html). Ознакомьтесь с [заявлением о конфиденциальности Майкрософт](https://aka.ms/csspeech/privacy).

Для выполнения приложений на разных платформах имеются различные зависимости.

## <a name="windows"></a>Windows

Пакет SDK для службы "Речь" в Cognitive Services протестирован для Windows 10 и Windows Server 2016.

Для работы Cognitive Services Speech SDK требуется [распространяемый C++ компонент Microsoft Visual Studio 2019](https://support.microsoft.com/help/2977003/the-latest-supported-visual-c-downloads) в системе. Установщики для последней версии `Microsoft Visual C++ Redistributable for Visual Studio 2019` можно скачать здесь:

- [Win32](https://aka.ms/vs/16/release/vc_redist.x86.exe)
- [x64](https://aka.ms/vs/16/release/vc_redist.x64.exe)

Если в приложении используется управляемый код, на целевом компьютере необходимо установить `.NET Framework 4.6.1` или более поздней версии.

Для ввода с микрофона необходимо установить библиотеки Media Foundation. Эти библиотеки входят в Windows 10 и Windows Server 2016. Пакет SDK для службы "Речь" можно использовать без этих библиотек, если в качестве аудиоустройства ввода не используется микрофон.

Необходимые файлы пакета SDK для службы "Речь" можно развернуть в том же каталоге, что и приложение. Таким образом, приложение сможет получать прямой доступ к библиотекам. Убедитесь, что выбрана правильная версия (Win32/x64), соответствующая приложению.

| Название | Функция
|:-----|:----|
| `Microsoft.CognitiveServices.Speech.core.dll` | Основной пакет SDK, необходимый для развертывания управляемого и машинного кода
| `Microsoft.CognitiveServices.Speech.csharp.dll` | Требуется для развертывания управляемого кода

>[!NOTE]
> Начиная с выпуска 1.3.0 файл `Microsoft.CognitiveServices.Speech.csharp.bindings.dll` (отгружен в предыдущих выпусках) больше не требуется. Теперь функциональность интегрирована в пакет SDK для Core.

>[!NOTE]
> Для проекта Windows Forms приложения (.NET Framework) C# убедитесь, что библиотеки включены в параметры развертывания проекта. Это можно проверить в разделе `Properties -> Publish Section`. `Application Files` Нажмите кнопку и найдите соответствующие библиотеки в списке Прокрутка вниз. Убедитесь, что для `Included`параметра задано значение. Visual Studio будет включать файл при публикации или развертывании проекта.

## <a name="linux"></a>Linux

В настоящее время пакет SDK для распознавания речи поддерживает дистрибутивы Ubuntu 16,04, Ubuntu 18,04 и Debian 9.
Для собственных приложений необходимо доставить библиотеку пакета SDK для службы "Речь", `libMicrosoft.CognitiveServices.Speech.core.so`.
Убедитесь, что выбрана правильная версия (x86, x64), соответствующая приложению. В зависимости от версии Linux может потребоваться включить следующие зависимости:

* общие библиотеки в библиотеке GNU C (включая библиотеку программирования потоков POSIX `libpthreads`);
* Библиотека OpenSSL (`libssl.so.1.0.0` или `libssl.so.1.0.2`)
* общую библиотеку для приложений ALSA (`libasound.so.2`)

В Ubuntu библиотеки GNU C должны быть уже установлены по умолчанию. Последние три библиотеки можно установить с помощью следующих команд:

```sh
sudo apt-get update
sudo apt-get install libssl1.0.0 libasound2
```

На Debian 9 Установите следующие пакеты:

```sh
sudo apt-get update
sudo apt-get install libssl1.0.2 libasound2
```

## <a name="next-steps"></a>Следующие шаги

* [Пробная версия Cognitive Services](https://azure.microsoft.com/try/cognitive-services/)
* [Распознавание речи в C#](quickstart-csharp-dotnet-windows.md)
