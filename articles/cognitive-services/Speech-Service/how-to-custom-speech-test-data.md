---
title: Подготовка тестовых данных для Пользовательское распознавание речи службы речи
titleSuffix: Azure Cognitive Services
description: Если вы тестируете, чтобы увидеть, насколько точны средства распознавания речи Майкрософт — или обучить собственные модели, вам понадобятся данные (в виде звука и (или) текста). На этой странице мы расскажем о типах данных, способах их использования и управлении ими.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 07/05/2019
ms.author: erhopf
ms.openlocfilehash: fad0fada4d9dd888b0b2a37b59e4eac1e016aec4
ms.sourcegitcommit: e3b0fb00b27e6d2696acf0b73c6ba05b74efcd85
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/30/2019
ms.locfileid: "68663601"
---
# <a name="prepare-data-for-custom-speech"></a>Подготовка данных для Пользовательское распознавание речи

Если вы тестируете, чтобы увидеть, насколько точны средства распознавания речи Майкрософт — или обучить собственные модели, вам понадобятся данные в виде аудио и текста. На этой странице мы расскажем о типах данных, способах их использования и управлении ими.

## <a name="data-types"></a>Типы данных

В этой таблице перечислены допустимые типы данных, когда следует использовать каждый тип данных и рекомендуемое количество. Для создания модели требуется не каждый тип данных. Требования к данным зависят от того, создаете ли вы тестирование или обучение модели.

| Тип данных | Используется для тестирования | Количество | Используется для обучения | Количество |
|-----------|-----------------|----------|-------------------|----------|
| [Звук:](#audio-data-for-testing) | Да<br>Используется для визуальной проверки | более 5 звуковых файлов | Нет | Н/д |
| [Звуковые и пользовательские записи с метками](#audio--human-labeled-transcript-data-for-testingtraining) | Да<br>Используется для вычисления точности | 0,5 – 5 часов звука | Да | 1 – 1 000 часов звука |
| [Связанный текст](#related-text-data-for-training) | Нет | Н/д | Да | 1-200 МБ связанного текста |

Файлы должны быть сгруппированы по типу в набор данных и переданы в виде ZIP-файла. Каждый набор данных может содержать только один тип данных.

## <a name="upload-data"></a>Отправка данных

Когда вы будете готовы отправить данные, нажмите кнопку **отправить данные** , чтобы запустить мастер и создать первый набор данных. Прежде чем разрешить передачу данных, вам будет предложено выбрать тип данных речи для набора данных.

![Выбор звука на портале речевого ввода](./media/custom-speech/custom-speech-select-audio.png)

Каждый передаваемый набор данных должен соответствовать требованиям к выбранному типу данных. Важно правильно отформатировать данные перед их отправкой. Это гарантирует, что данные будут правильно обрабатываться службой Пользовательское распознавание речи. Требования перечислены в следующих разделах.

После отправки набора данных существует несколько вариантов:

* Вы можете переходить на вкладку " **тестирование** " и визуально проверять только звуковые данные или аудиозаписи с метками "звук" и "человек".
* Вы можете переходить на вкладку " **обучение** " и использовать звуковые и видеозаписи или связанные текстовые данные для обучения настраиваемой модели.

## <a name="audio-data-for-testing"></a>Звуковые данные для тестирования

Звуковые данные оптимальны для тестирования точности базовой модели преобразования речи в текст или настраиваемой модели корпорации Майкрософт. Помните, что звуковые данные используются для проверки точности речи с точки зрения производительности конкретной модели. Если вы хотите количественно определить точность модели, используйте [звуковые и человеческие данные](#audio--human-labeled-transcript-data-for-testingtraining)для транскрипции.

Используйте эту таблицу, чтобы убедиться, что звуковые файлы правильно отформатированы для использования с Пользовательское распознавание речи.

| Свойство | Значение |
|----------|-------|
| Формат файла | RIFF (WAV) |
| Частота выборки | 8 000 Гц или 16 000 Гц |
| Каналы | 1 (моно) |
| Максимальная длина на одном аудио | 2 часа |
| Формат выборки | PCM, 16-разрядные |
| Формат архива | .zip |
| Максимальный размер архива | 2 ГБ |

Если ваш звук не удовлетворяет этим свойствам или вы хотите проверить их, мы рекомендуем скачать [Sox](http://sox.sourceforge.net) для проверки или преобразования звука. Ниже приведены некоторые примеры того, как можно выполнить каждое из этих действий в командной строке:

| Действия | Описание | Команда Sox |
|----------|-------------|-------------|
| Проверить формат звука | Используйте эту команду для проверки формата звукового файла. | `sox --i <filename>` |
| Преобразовать формат аудио | Используйте эту команду, чтобы преобразовать звуковой файл в один канал, 16-разрядный, 16 кГц. | `sox <input> -b 16 -e signed-integer -c 1 -r 16k -t wav <output>.wav` |

## <a name="audio--human-labeled-transcript-data-for-testingtraining"></a>Звуковые и человеческие данные для тестирования и обучения

Чтобы измерить точность точности речи в тексте от корпорации Майкрософт при обработке звуковых файлов, для сравнения необходимо предоставить транскрипции с метками (по словам). Хотя транскрипции с метками часто потребляют много времени, необходимо оценить точность и обучить модель для вариантов использования. Помните, что улучшения распознавания будут так же хороши, как и предоставленные данные. По этой причине важно, чтобы отправлялись только высококачественные записи.  

| Свойство | Значение |
|----------|-------|
| Формат файла | RIFF (WAV) |
| Частота выборки | 8 000 Гц или 16 000 Гц |
| Каналы | 1 (моно) |
| Максимальная длина на одном аудио | 60 с |
| Формат выборки | PCM, 16-разрядные |
| Формат архива | .zip |
| Максимальный размер ZIP-файла | 2 ГБ |

Для устранения таких проблем, как удаление или замена Word, требуется значительный объем данных для улучшения распознавания. Как правило, рекомендуется предоставлять записи в формате Word примерно в 10 – 1 000 часов. Транскрипции всех WAV-файлов должны содержаться в одном текстовом файле. Каждая строка файла транскрипции должна содержать имя звукового файла и соответствующую ему транскрипцию. Для разделения имени файла и транскрипции необходимо использовать символ табуляции (\t).

  Пример:
```
  speech01.wav  speech recognition is awesome
  speech02.wav  the quick brown fox jumped all over the place
  speech03.wav  the lazy dog was not amused
```
> [!NOTE]
> Для кодировки транскрипции необходимо использовать метку порядка байтов UTF-8.

Текст транскрипции нормализуется для эффективной обработки системой. Однако есть некоторые важные действия нормализации, которые пользователь должен выполнять _перед_ отправкой данных в пользовательскую службу распознавания речи. Сведения о том, как использовать соответствующий язык при подготовке транскрипции, см. в разделе [Создание транскрипции](how-to-custom-speech-human-labeled-transcriptions.md) с отметкой

После сбора звуковых файлов и соответствующих транскрипций их следует упаковать как один ZIP-файл перед отправкой на портал Пользовательское распознавание речи. Это пример набора данных с тремя звуковыми файлами и файлом транскрипции с меткой:

![Выбор звука на портале речевого ввода](./media/custom-speech/custom-speech-audio-transcript-pairs.png)

## <a name="related-text-data-for-training"></a>Связанные текстовые данные для обучения

Если у вас есть уникальные имена продуктов или компоненты и вы хотите убедиться, что они правильно распознаны, важно включить связанные текстовые данные для обучения. Для улучшения распознавания можно предоставить два типа связанных текстовых данных:

| Тип данных | Как эти данные улучшают распознавание |
|-----------|------------------------------------|
| Фразы продолжительностью и/или предложения | Они могут улучшить точность при распознавании названий продуктов или отраслевых словарей в контексте предложения. |
| Произношение | Они могут улучшить произношение нестандартных терминов, акронимов или других слов с неопределенными произношениями. |

Фразы продолжительностью может быть предоставлен как один или несколько текстовых файлов. Чем ближе текстовые данные к тем, что будет произнесено, тем выше вероятность повышения точности. Произношение следует указывать в виде одного текстового файла. Все может быть упаковано в один ZIP-файл и отправлено на портал Пользовательское распознавание речи.

### <a name="guidelines-to-create-an-utterances-file"></a>Рекомендации по созданию файла фразы продолжительностью

Чтобы создать пользовательскую модель с помощью связанного текста, необходимо предоставить список примеров фразы продолжительностью. Эти фразы продолжительностью не обязательно должны быть верными предложениями или грамматически правильными, но они должны точно отражать речевой ввод, который вы ждете в рабочей среде. Если вы хотите, чтобы определенные термины имели более высокую плотность, можно добавить в связанный файл данных несколько предложений, включающих эти конкретные условия.

Используйте эту таблицу, чтобы убедиться, что связанный файл данных для фразы продолжительностью имеет правильный формат:

| Свойство | Значение |
|----------|-------|
| Кодировка текста | Метка порядка байтов UTF-8 |
| Количество фраз в строке | 1 |
| Максимальный размер файла | 200 МБ |

Кроме того, необходимо учитывать следующие ограничения.

* Старайтесь не повторять повторяющиеся символы более четырех раз. Например: "AAAA" или "УУУУ".
* Не используйте специальные символы или символы UTF-8 выше U + 00A1.
* URI будут отклонены.

### <a name="guidelines-to-create-a-pronunciation-file"></a>Рекомендации по созданию файла произношения

При наличии редко встречающихся условий без использования стандартных произношений, которые будут встречаться или использовать пользователи, можно предоставить пользовательский файл произношения для улучшения распознавания.

> [!IMPORTANT]
> Не рекомендуется использовать эту функцию для изменения произношения часто встречающихся слов.

Сюда входят примеры произнесенного utteranceа и пользовательское произношение для каждого из них:

| Распознанная или отображаемая форма | Речевая форма |
|--------------|--------------------------|
| 3CPO | три c p o |  
| CNTK | c n t k |
| СТАНДАРТ | я тройной e |

Произнесенная форма — это фонетическая последовательность с буквами. Оно может состоять из букв, слов, слогов или сочетания всех трех.

Настройка произношения доступна на английском языке (EN-US) и немецком (de-DE). В этой таблице показаны поддерживаемые символы по языку:

| Язык | Языковые стандарты | Символы |
|----------|--------|------------|
| Английский | ru-RU | а, b, c, d, e, f, g, h, i, j, k, l, m, n, o, p, q, r, s, t, u, v, w, x, y, z |
| Немецкий | de-DE | ä, ц, u, a, b, c, d, e, f, g, h, i, j, k, l, m, n, o, p, q, r, s, t, u, v, w, x, y, z |

Используйте эту таблицу, чтобы убедиться, что связанный файл данных для произношения отформатирован правильно. Файлы произношения имеют небольшой размер и не должны превышать несколько KBs.

| Свойство | Значение |
|----------|-------|
| Кодировка текста | Спецификация UTF-8 (для английского языка также поддерживается ANSI) |
| число произношений в строке | 1 |
| Максимальный размер файла | 1 МБ (1 КБ для уровня Free) |

## <a name="next-steps"></a>Следующие шаги

* [Проверка данных](how-to-custom-speech-inspect-data.md)
* [Оценка данных](how-to-custom-speech-evaluate-data.md)
* [Обучение модели](how-to-custom-speech-train-model.md)
* [Развертывание модели](how-to-custom-speech-deploy-model.md)
