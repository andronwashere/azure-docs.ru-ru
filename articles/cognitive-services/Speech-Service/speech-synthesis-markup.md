---
title: Язык разметки речи (SSML) — служба речи
titleSuffix: Azure Cognitive Services
description: С помощью языка Speech Synthesis Markup Language можно управлять произношением и интонацией при преобразовании текста в речь.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 07/05/2019
ms.author: erhopf
ms.openlocfilehash: 1bd1882218630aca0707a792d120045c06dea127
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/26/2019
ms.locfileid: "68552677"
---
# <a name="speech-synthesis-markup-language-ssml"></a>Язык разметки синтеза речи (SSML)

Язык разметки речи (SSML) — это язык разметки на основе XML, позволяющий разработчикам указать способ преобразования текста ввода в синтезированное речевое сообщение с помощью службы "преобразование текста в речь". По сравнению с обычным текстом, SSML позволяет разработчикам точно настраивать тон, произношение, скорость речи, громкость и другие выходные данные текста в речь. Обычные знаки препинания, такие как пауза после точки, или использование правильного интонатион, когда предложение заканчивается вопросительным знаком, автоматически обрабатывается.

Реализация службы распознавания речи SSML основана на языке разметки для [распознавания речи консорциум W3C версии 1,0](https://www.w3.org/TR/speech-synthesis).

> [!IMPORTANT]
> Символы китайского, японского и корейского языков считаются двумя символами для выставления счетов. Дополнительные сведения см. на странице [цен](https://azure.microsoft.com/pricing/details/cognitive-services/speech-services/).

## <a name="standard-neural-and-custom-voices"></a>Стандартные, нейронные и пользовательские голоса

Выберите один из стандартных и нейронных голосов или создайте собственный пользовательский голос, уникальный для продукта или торговой марки. более 75 языков и национальных стандартов доступны на более чем 45 языках, а на 4 языках и национальных стандартах доступно 5 голосов нейронов. Полный список поддерживаемых языков, языковых стандартов и голосовых моделей (нейронных и стандартных) см. в разделе [языковой поддержки](language-support.md).

Дополнительные сведения о стандартном, нейронном и настраиваемом голосовом см. в статье [Обзор преобразования текста в речь](text-to-speech.md).

## <a name="supported-ssml-elements"></a>Поддерживаемые элементы SSML

Каждый документ SSML создается с помощью элементов SSML (или тегов). Эти элементы используются для настройки высоты, Prosody, громкости и т. д. В следующих разделах подробно описано, как используется каждый элемент, а также когда элемент является обязательным или необязательным.  

> [!IMPORTANT]
> Не забывайте использовать двойные кавычки вокруг значений атрибутов. Стандарты для правильного формата, допустимые XML-данные, требуют, чтобы значения атрибутов заключались в двойные кавычки. Например, `<prosody volume="90">` является правильно сформированным, допустимым элементом, но `<prosody volume=90>` не имеет значение. SSML может не распознать значения атрибутов, которые не находятся в кавычках.

## <a name="create-an-ssml-document"></a>Создание документа SSML

`speak`является корневым элементом и является **обязательным** для всех документов SSML. `speak` Элемент содержит важную информацию, например версию, язык и определение словаря разметки.

**Синтаксис**

```xml
<speak version="1.0" xmlns="https://www.w3.org/2001/10/synthesis" xml:lang="string"></speak>
```

**Атрибуты**

| Атрибут | Описание | Обязательный или необязательный |
|-----------|-------------|---------------------|
| version | Указывает версию спецификации SSML, используемую для интерпретации разметки документа. Текущая версия — 1,0. | Обязательное значение |
| XML: lang | Указывает язык корневого документа. Значение может содержать символы в нижнем регистре, двухбуквенный код языка (например, **EN**), языковой код и страну или регион в верхнем регистре (например, **en-US**). | Обязательное значение |
| xmlns | Задает универсальный код ресурса (URI) документа, который определяет словарь разметки (типы элементов и имена атрибутов) документа SSML. Текущий URI — https://www.w3.org/2001/10/synthesis. | Обязательное значение |

## <a name="choose-a-voice-for-text-to-speech"></a>Выбор голоса для преобразования текста в речь

`voice` Элемент является обязательным. Он используется для указания голоса, используемого для преобразования текста в речь.

**Синтаксис**

```xml
<voice name="string">
    This text will get converted into synthesized speech.
</voice>
```

**Атрибуты**

| Атрибут | Описание | Обязательный или необязательный |
|-----------|-------------|---------------------|
| name | Определяет голос, используемый для вывода текста в речь. Полный список поддерживаемых голосов см. в разделе [Поддержка языков](language-support.md#text-to-speech). | Обязательное значение |

**Пример**

> [!NOTE]
> В `en-US-Jessa24kRUS` этом примере используется речь. Полный список поддерживаемых голосов см. в разделе [Поддержка языков](language-support.md#text-to-speech).

```XML
<speak version="1.0" xmlns="https://www.w3.org/2001/10/synthesis" xml:lang="en-US">
    <voice  name="en-US-Jessa24kRUS">
        This is the text that is spoken.
    </voice>
</speak>
```

## <a name="use-multiple-voices"></a>Использование нескольких голосов

`speak` В элементе можно указать несколько голосов для вывода текста в речь. Эти голоса могут быть на разных языках. Для каждого голоса текст должен быть заключен в `voice` оболочку элемента.

**Атрибуты**

| Атрибут | Описание | Обязательный или необязательный |
|-----------|-------------|---------------------|
| name | Определяет голос, используемый для вывода текста в речь. Полный список поддерживаемых голосов см. в разделе [Поддержка языков](language-support.md#text-to-speech). | Обязательное значение |

**Пример**

```xml
<speak version="1.0" xmlns="https://www.w3.org/2001/10/synthesis" xml:lang="en-US">
    <voice  name="en-US-Jessa24kRUS">
        Good morning!
    </voice>
    <voice  name="en-US-Guy24kRUS">
        Good morning to you too Jessa!
    </voice>
</speak>
```

## <a name="adjust-speaking-styles"></a>Изменить стили речи

> [!IMPORTANT]
> Эта функция будет работать только с нейронными голосовыми данными.

По умолчанию в службе преобразования текста в речь используется нейтральный стиль для стандартных и нейронных голосов. С помощью нейронных голосов можно настроить стиль речи, чтобы выразить чирфулнесс, сопереживание или тональности с `<mstts:express-as>` помощью элемента. Это необязательный элемент, уникальный для служб распознавания речи Azure.

В настоящее время для этих нейронных голосов поддерживаются настройки стиля речи:
* `en-US-JessaNeural`
* `zh-CN-XiaoxiaoNeural`

Изменения применяются на уровне предложений, а стиль зависит от голоса. Если стиль не поддерживается, служба вернет речь по нейтральному стилю речи по умолчанию.

**Синтаксис**

```xml
<mstts:express-as type="string"></mstts:express-as>
```

**Атрибуты**

| Атрибут | Описание | Обязательный или необязательный |
|-----------|-------------|---------------------|
| type | Задает стиль речи. В настоящее время стиль речи зависит от голоса. | Требуется при настройке стиля речи для нейронной речи. Если используется `mstts:express-as`, необходимо указать тип. Если указано недопустимое значение, этот элемент будет проигнорирован. |

Используйте эту таблицу, чтобы определить, какие стили речи поддерживаются для каждого нейронного голоса.

| Голос | Тип | Описание |
|-------|------|-------------|
| `en-US-JessaNeural` | Тип =`cheerful` | Выражает положительное и счастливое распознавания эмоций |
| | Тип =`empathy` | Выражает представление о волнует и понимании |
| `zh-CN-XiaoxiaoNeural` | Тип =`newscast` | Выражает формальный тон аналогично новостным трансляциям |
| | Тип =`sentiment` | Передает сообщение касания или историю |

**Пример**

В этом фрагменте SSML показано `<mstts:express-as>` , как элемент используется для изменения стиля речи на `cheerful`.

```xml
<speak version="1.0" xmlns="https://www.w3.org/2001/10/synthesis" xmlns:mstts="https://www.w3.org/2001/mstts" xml:lang="en-US">
    <voice name="en-US-JessaNeural">
        <mstts:express-as type="cheerful">
            That'd be just amazing!
        </mstts:express-as>
    </voice>
</speak>
```

## <a name="add-or-remove-a-breakpause"></a>Добавление или удаление перерыва или паузы

`break` Используйте элемент, чтобы вставить паузы (или перерывы) между словами или предотвратить автоматическое добавление приостанова службой преобразования текста в речь.

> [!NOTE]
> Используйте этот элемент, чтобы переопределить поведение по умолчанию преобразования текста в речь (TTS) для слова или фразы, если синтезированное речевое выражение для этого слова или фразы звучит неестественно. Задайте значение `strength` , чтобыпредотвратитьразрывинтонационную,которыйавтоматическивставляетсяслужбойпреобразованиятекставречь.`none`

**Синтаксис**

```xml
<break strength="string" />
<break time="string" />
```

**Атрибуты**

| Атрибут | Описание | Обязательный или необязательный |
|-----------|-------------|---------------------|
| сложность | Задает относительную продолжительность паузы, используя одно из следующих значений:<ul><li>none</li><li>x-слабый</li><li>низкая</li><li>Средний (по умолчанию)</li><li>высокая</li><li>строгость x</li></ul> | Необязательный |
| time | Задает абсолютную длительность паузы в секундах или миллисекундах. Примеры допустимых значений — от 2 до 500. | Необязательный |

| Сложность | Описание |
|----------|-------------|
| None или, если значение не указано | 0 мс |
| x-слабый | 250 мс |
| низкая | 500 мс |
| средняя | 750 мс |
| высокая | 1000 мс |
| строгость x | 1250 МС |


**Пример**

```xml
<speak version="1.0" xmlns="https://www.w3.org/2001/10/synthesis" xml:lang="en-US">
    <voice  name="en-US-Jessa24kRUS">
        Welcome to Microsoft Cognitive Services <break time="100ms" /> Text-to-Speech API.
    </voice>
</speak>
```

## <a name="specify-paragraphs-and-sentences"></a>Указание абзацев и предложений

`p`элементы `s` and используются для обозначения абзацев и предложений соответственно. При отсутствии этих элементов служба преобразования текста в речь автоматически определяет структуру документа SSML.

`prosody` `break` `phoneme` `audio` `sub` `mstts:express-as`Элемент может содержать текст и следующие элементы:,,,, `say-as`,, и. `s` `p`

`phoneme` `break` `audio` `prosody` Элементможет`say-as`содержать текст и следующие элементы :`mstts:express-as`,,,,, и`sub`. `s`

**Синтаксис**

```XML
<p></p>
<s></s>
```

**Пример**

```XML
<speak version="1.0" xmlns="https://www.w3.org/2001/10/synthesis" xml:lang="en-US">
    <voice  name="en-US-Jessa24kRUS">
        <p>
            <s>Introducing the sentence element.</s>
            <s>Used to mark individual sentences.</s>
        </p>
        <p>
            Another simple paragraph.
            Sentence structure in this paragraph is not explicitly marked.
        </p>
    </voice>
</speak>
```

## <a name="use-phonemes-to-improve-pronunciation"></a>Использование фонемы для улучшения произношения

`ph` Элемент используется для фонетического произношения в документах SSML. `ph` Элемент может содержать только текст, но не другие элементы. Всегда предоставляющих удобное для восприятия речь в качестве резерва.

Фонетические алфавиты состоят из телефонов, которые состоят из букв, цифр или символов, иногда в сочетании. Каждый телефон описывает уникальный звук речи. Это отличается от латинского алфавита, где любая буква может представлять несколько речевых звуков. Рассмотрите разные произношения буквы "c" в словах "«а» и «прекращение» или другие произношение буквы «TH» в словах «вещь» и «эти».

**Синтаксис**

```XML
<phoneme alphabet="string" ph="string"></phoneme>
```

**Атрибуты**

| Атрибут | Описание | Обязательный или необязательный |
|-----------|-------------|---------------------|
| буквы | Указывает фонетический алфавит, используемый при создании транскрипции строки в `ph` атрибуте. Строка, указывающая букву алфавита, должна быть указана в строчных буквах. Ниже приведены возможные алфавиты, которые можно указать.<ul><li>IPA &ndash; Международный фонетический алфавит</li><li>набор &ndash; телефонного API-интерфейса SAPI Speech</li><li>набор &ndash; универсальных телефонов ИБП</li></ul>Алфавит применяется только к phoneme в элементе. Дополнительные сведения см. в разделе [Справочник](https://msdn.microsoft.com/library/hh362879(v=office.14).aspx)по фонетическому алфавиту. | Необязательный |
| Фаза | Строка, содержащая телефоны, задающих произношение слова в `phoneme` элементе. Если указанная строка содержит нераспознаваемые телефоны, служба преобразования текста в речь (TTS) отклоняет весь документ SSML и не выдает ни одного из речевых выходных данных, указанных в документе. | Требуется при использовании фонемы. |

**Примеры**

```XML
<speak version="1.0" xmlns="https://www.w3.org/2001/10/synthesis" xml:lang="en-US">
    <voice  name="en-US-Jessa24kRUS">
        <s>His name is Mike <phoneme alphabet="ups" ph="JH AU"> Zhou </phoneme></s>
    </voice>
</speak>
```

```xml
<speak version="1.0" xmlns="https://www.w3.org/2001/10/synthesis" xml:lang="en-US">
    <voice  name="en-US-Jessa24kRUS">
        <phoneme alphabet="ipa" ph="t&#x259;mei&#x325;&#x27E;ou&#x325;"> tomato </phoneme>
    </voice>
</speak>
```

## <a name="adjust-prosody"></a>Настройка Prosody

`prosody` Элемент используется для указания изменений в шагах, каунтаур, диапазоне, скорости, длительности и том для вывода текста в речь. `phoneme` `break` `p` `audio` `say-as` `sub`Элемент может содержать текст и следующие элементы:,,,, `prosody`,, и. `s` `prosody`

Так как значения атрибутов интонационную могут различаться для широкого диапазона, распознаватель речи интерпретирует назначенные значения как предложение фактического значения интонационную выбранного голоса. Служба преобразования текста в речь ограничивает или заменяет значения, которые не поддерживаются. Примеры неподдерживаемых значений — шаг 1 МГц или объем 120.

**Синтаксис**

```XML
<prosody pitch="value" contour="value" range="value" rate="value" duration="value" volume="value"></prosody>
```

**Атрибуты**

| Атрибут | Описание | Обязательный или необязательный |
|-----------|-------------|---------------------|
| тон | Указывает шаг по базовому плану для текста. Вы можете выразить тон следующим образом:<ul><li>Абсолютное значение, выраженное в виде числа, за которым следует "Гц" (герц). Например, 600Hz.</li><li>Относительное значение, выраженное в виде числа, начинающегося с "+" или "-" и заканчивающееся на "Гц" или "St", которое указывает величину изменения высоты. Например: + 80Hz или-2st. "St" означает, что единица изменения — полтона, то есть половина тона (половина шага) на стандартной диатоник шкале.</li><li>Значение константы:<ul><li>x-низкий</li><li>низкий</li><li>средняя</li><li>высокий</li><li>x-высокий</li><li>значение по умолчанию</li></ul></li></ul>. | Необязательный |
| профилей | Профиль не поддерживается для нейронных голосов. Профиль представляет изменения в пошаговом речевом содержимом в виде массива целевых объектов в заданные положения речевого ввода. Каждый целевой объект определяется набором пар параметров. Пример: <br/><br/>`<prosody contour="(0%,+20Hz) (10%,-2st) (40%,+10Hz)">`<br/><br/>Первое значение в каждом наборе параметров указывает расположение изменения высоты в процентах от длительности текста. Второе значение задает величину, которую нужно увеличить или уменьшить, используя относительное значение или значение перечисления для высоты (см `pitch`.). | Необязательный |
| range  | Значение, представляющее диапазон высоты для текста. `range` Для описания`pitch`можно использовать те же абсолютные значения, относительные значения или значения перечисления. | Необязательный |
| курсе  | Указывает интенсивность речи текста. Вы можете выразить `rate` следующим образом:<ul><li>Относительное значение, выраженное в виде числа, которое выступает в качестве множителя по умолчанию. Например, значение *1* приводит к отсутствию изменений в скорости. Значение *0,5* приводит к половине скорости. Значение *3* приводит к утраиваетсяу скорости.</li><li>Значение константы:<ul><li>x-замедляют</li><li>медленный</li><li>средняя</li><li>ускоряет</li><li>x-Fast</li><li>значение по умолчанию</li></ul></li></ul> | Необязательный |
| duration  | Период времени, в течение которого служба синтеза речи (TTS) считывает текст в секундах или миллисекундах. Например, *2S* или *1800ms*. | Необязательный |
| том  | Указывает уровень громкости речи. Том можно выразить следующим образом:<ul><li>Абсолютное значение, выраженное в виде числа в диапазоне от 0,0 до 100,0 *, от самого* дальнего к *громкости*. Например, 75. Значение по умолчанию — 100,0.</li><li>Относительное значение, выраженное в виде числа с префиксом "+" или "-", которое указывает количество изменений в томе. Например, + 10 или-5,5.</li><li>Значение константы:<ul><li>Удаля</li><li>x — Soft</li><li>мягкий</li><li>средняя</li><li>сделать</li><li>x-вслух</li><li>значение по умолчанию</li></ul></li></ul> | Необязательный |

### <a name="change-speaking-rate"></a>Изменение скорости речи

Скорость речи можно применять к стандартным голосовым данным на уровне слов или предложений. В то время как скорость речи может применяться только к нейронным голосовым уровням на уровне предложений.

**Пример**

```xml
<speak version="1.0" xmlns="https://www.w3.org/2001/10/synthesis" xml:lang="en-US">
    <voice  name="en-US-Guy24kRUS">
        <prosody rate="+30.00%">
            Welcome to Microsoft Cognitive Services Text-to-Speech API.
        </prosody>
    </voice>
</speak>
```

### <a name="change-volume"></a>Изменение громкости

Изменения тома можно применить к стандартным голосовым данным на уровне слов или предложений. Изменения тома могут применяться только к нейронным голосовым уровням на уровне предложений.

**Пример**

```xml
<speak version="1.0" xmlns="https://www.w3.org/2001/10/synthesis" xml:lang="en-US">
    <voice  name="en-US-Jessa24kRUS">
        <prosody volume="+20.00%">
            Welcome to Microsoft Cognitive Services Text-to-Speech API.
        </prosody>
    </voice>
</speak>
```

### <a name="change-pitch"></a>Изменение тона

Изменения высоты можно применить к стандартным голосовым параметрам на уровне слов или предложений. Изменения высоты могут применяться только к нейронным голосовым уровням на уровне предложений.

**Пример**

```xml
<speak version="1.0" xmlns="https://www.w3.org/2001/10/synthesis" xml:lang="en-US">
    <voice  name="en-US-Guy24kRUS">
        Welcome to <prosody pitch="high">Microsoft Cognitive Services Text-to-Speech API.</prosody>
    </voice>
</speak>
```

### <a name="change-pitch-contour"></a>Изменение контура тона

> [!IMPORTANT]
> Изменения профиля не поддерживаются для нейронных голосов.

**Пример**

```xml
<speak version="1.0" xmlns="https://www.w3.org/2001/10/synthesis" xml:lang="en-US">
    <voice  name="en-US-Jessa24kRUS">
        <prosody contour="(80%,+20%) (90%,+30%)" >
            Good morning.
        </prosody>
    </voice>
</speak>
```

## <a name="add-recorded-audio"></a>Добавить записанный звук

`audio`— Это необязательный элемент, позволяющий вставлять звук MP3 в документ SSML. Тело элемента audio может содержать обычный текст или SSML разметку, которые говорят, если звуковой файл недоступен или воспроизводится. Кроме того, `audio` элемент может содержать текст и следующие элементы: `audio`, `break`, `p`, `s`, `phoneme`, `prosody`, `say-as`и `sub`.

Все аудио, входящие в документ SSML, должны соответствовать следующим требованиям:

* MP3 должен размещаться на доступной в Интернете конечной точке HTTPS. Требуется протокол HTTPS, а домен, на котором размещается MP3-файл, должен представлять действительный доверенный SSL-сертификат.
* Формат MP3 должен быть допустимым MP3-файлом (MPEG v2).
* Скорость потока должна составлять 48 кбит/с.
* Частота дискретизации должна составлять 16000 Гц.
* Суммарное общее время для всех текстовых и звуковых файлов в одном ответе не может превышать 90 (90) секунд.
* MP3 не должен содержать никаких конфиденциальных сведений, относящихся к заказчику.

**Синтаксис**

```xml
<audio src="string"/></audio>
```

**Атрибуты**

| Атрибут | Описание | Обязательный или необязательный |
|-----------|-------------|---------------------|
| src | Указывает расположение или URL-адрес звукового файла. | Требуется при использовании элемента audio в документе SSML. |

**Пример**

```xml
<speak version="1.0" xmlns="https://www.w3.org/2001/10/synthesis" xml:lang="en-US">
    <p>
        <audio src="https://contoso.com/opinionprompt.wav"/>
        Thanks for offering your opinion. Please begin speaking after the beep.
        <audio src="https://contoso.com/beep.wav">
        Could not play the beep, please voice your opinion now. </audio>
    </p>
</speak>
```

## <a name="add-background-audio"></a>Добавить фоновый звук

`mstts:backgroundaudio` Элемент позволяет добавить фоновый звук в документы SSML (или смешать звуковой файл с текстом в речь). С `mstts:backgroundaudio` помощью можно перебрать звуковой файл в фоновом режиме, появление в начале текста в речь и появление в конце текста в речь.

Если предоставленный фоновый звук короче, чем преобразование текста в речь или Исчезание, он будет циклическим. Если оно длиннее, чем преобразование текста в речь, оно будет остановлено по завершении затухания.

Для каждого документа SSML допускается только один фоновый звуковой файл. Тем не менее `audio` теги `voice` можно добавлять в элемент, чтобы добавить дополнительный звук в документ SSML.

**Синтаксис**

```XML
<mstts:backgroundaudio src="string" volume="string" fadein="string" fadeout="string"/>
```

**Атрибуты**

| Атрибут | Описание | Обязательный или необязательный |
|-----------|-------------|---------------------|
| src | Указывает расположение или URL-адрес фонового звукового файла. | Требуется при использовании фонового звука в документе SSML. |
| том | Указывает громкость фонового звукового файла. **Допустимые значения** `0` : `100` в включительно. Значение по умолчанию — `1`. | Необязательный |
| FadeIn | Указывает длительность фонового исчезновения звука. **Допустимые значения** `0` : `10000` в включительно.  | Необязательный |
| FadeOut | Указывает длительность фонового исчезновения звука. **Допустимые значения** `0` : `10000` в включительно.  | Необязательный |

**Пример**

```xml
<speak version="1.0" xml:lang="en-US" xmlns:mstts="http://www.w3.org/2001/mstts">
    <mstts:backgroundaudio src="https://contoso.com/sample.wav" volume="0.7" fadein="3000" fadeout="4000"/>
    <voice name="Microsoft Server Speech Text to Speech Voice (en-US, Jessa24kRUS)">
        The text provided in this document will be spoken over the background audio.
    </voice>
</speak>
```

## <a name="next-steps"></a>Следующие шаги

* [Поддержка языков и регионов в API службы "Речь"](language-support.md)
