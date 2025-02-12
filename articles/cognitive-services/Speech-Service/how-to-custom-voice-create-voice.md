---
title: Создание настраиваемых голосовых - службы распознавания речи
titlesuffix: Azure Cognitive Services
description: Когда вы будете готовы к отправке данных, перейдите на портал настраиваемые голоса. Создайте или выберите проект настраиваемый голос. Проект необходимо предоставить правой языка и языкового стандарта и пол свойства данных вам нужно использовать для обучения голоса.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 07/05/2019
ms.author: erhopf
ms.openlocfilehash: 0c2889cef01345f00d1669e7549a2fe25ac07b97
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2019
ms.locfileid: "67603122"
---
# <a name="create-a-custom-voice"></a>Создание настраиваемых голосовых

В [Подготовка данных для настраиваемых голосовых](how-to-custom-voice-prepare-data.md), мы описали различные типы данных можно использовать для обучения, настраиваемых голосовых и требования к другой формат. После подготовки данных, можно начать отправлять их для [голосовой пользовательский портал](https://aka.ms/custom-voice-portal), или через API обучения настраиваемый голос. Здесь мы расскажем обучения настраиваемых голосовых через портал.

> [!NOTE]
> Этой странице предполагается, что вы прочитали [приступить к работе с настраиваемый голос](how-to-custom-voice.md) и [Подготовка данных для пользовательских голосовых](how-to-custom-voice-prepare-data.md)и был создан проект настраиваемый голос.

Проверьте языков, поддерживаемых для настраиваемых голосовых: [язык для настройки](language-support.md#customization).

## <a name="upload-your-datasets"></a>Обновление наборов данных

Когда вы будете готовы к отправке данных, перейдите к [голосовой пользовательский портал](https://aka.ms/custom-voice-portal). Создайте или выберите проект настраиваемый голос. Проект необходимо предоставить правой языка и языкового стандарта и пол свойства данных вам нужно использовать для обучения голоса. Например, выберите `en-GB` Если аудиозаписи, у вас есть делается на английском языке с помощью часть соединенного Королевства, с учетом диакритических знаков.

Перейдите к **данных** вкладку и нажмите кнопку **отправка данных**. В мастере выберите правильный тип данных, соответствующий вы подготовили.

Каждый набор данных, которые вы отправляете должен соответствовать требованиям для выбранного типа данных. Очень важно правильно отформатировать данные перед отправкой. Это гарантирует, что данные будут обрабатываться службой настраиваемый голос точно. Перейдите к [Подготовка данных для пользовательских голосовых](how-to-custom-voice-prepare-data.md) и убедитесь, что ваши данные правильно отформатирован.

> [!NOTE]
> Бесплатная подписка (F0) пользователи могут отправлять два набора данных одновременно. Стандартная подписка (S0) пользователи могут отправлять пяти наборов данных одновременно. Если вы достигли предела, подождите, пока завершится импортирование хотя бы одного из наборов данных. Затем повторите попытку.

> [!NOTE]
> Максимальное число наборов данных, которые разрешено импортировать на одну подписку — 10 ZIP-файл файлы бесплатно пользователи подписки (F0) и 500 для стандартной подписки (S0) пользователей.

Наборы данных проверяются автоматически после нажатия кнопки отправки. Проверка данных включает в себя серию проверок звуковые файлы, чтобы проверить их формат файла, размер и частоту выборки. Исправьте ошибки, если таковые имеются и повторите попытку. Успешно отправляет запрос на импорт данных, должна появиться запись в таблице данных, соответствующий набор данных, которые вы только что отправили.

В следующей таблице приведены состояния обработки импортированных наборов данных.

| Состояние | Значение |
| ----- | ------- |
| Обработка | Набор данных был получен и обрабатывается. |
| Succeeded | Набор данных был проверен и теперь может использоваться для построения модели голоса. |
| Сбой | Набор данных завершилась с ошибкой во время обработки вызвано разными причинами, например файл ошибок, проблем и проблемы с сетью. |

После завершения проверки можно увидеть общее число соответствующих фразы для каждого из наборов данных в **фразы** столбца. Если тип данных, которые вы выбрали требует сегментации долго аудио, этот столбец только отражает фразы, которые мы сегментированных автоматически, на основе табель или через службу транскрибирование речи. Дополнительно можно загрузить набор данных, проверки, чтобы просмотреть результаты детализации фразы успешно импортированы, а также их сопоставление табель. Подсказка: long-audio сегментации может занять больше часа для завершения обработки данных.

Для наборов данных en US "и" zh-CN Дополнительно можно загрузить отчета для проверки для каждой из записей оценки произношение и уровень шума. Оценка произношению дается в диапазоне от 0 до 100. Оценка ниже 70 обычно означает ошибку в речи или несоответствие в сценарии. Заметный акцент уменьшает оценку произношения и влияет на созданный цифровой голос.

Более высокий коэффициент сигнала и шума (SNR) обозначает более низкий уровень шума в звуковом файле. Обычно SNR выше 50 можно достичь, производя запись в профессиональных студиях. Звуковой файл с SNR менее 20 может привести к явному шуму в созданном голосе.

Рассмотрите возможность повторной записи любых высказываний с низкой оценкой произношения или слабым SNR. Если повторная запись невозможна, исключите эти высказывания из набора данных.

## <a name="build-your-custom-voice-model"></a>Создавать пользовательские голосовые модели

После проверки набора данных можно использовать его для создания пользовательские голосовые модели.

1.  Перейдите к **преобразования текста в речь > пользовательские голосовые > обучения**.

2.  Нажмите кнопку **Обучение модели**.

3.  Затем введите **имя** и **описание** для идентификации этой модели.

    Тщательно выбирайте имя. Имя, которое здесь вводится, будет использоваться, чтобы указать голос в запросе на синтез речи, как часть входных данных SSML. Только буквы, цифры и несколько знаков препинания, таких как - \_и («",»") разрешены. Используйте разные имена для различных голосовых моделей.

    В поле **Description** (Описание) обычно записываются имена наборов данных, которые используются для создания модели.

4.  Из **выберите обучающие данные** выберите один или несколько наборов данных, который вы хотите использовать для обучения. Проверьте количество фразы продолжительностью до их отправки. Можно начать с любым количеством фразы для en US "и" zh-CN голосовых моделей. Для других языков необходимо выбрать более 2 000 фразы, чтобы иметь возможность обучения голос.

    > [!NOTE]
    > Повторяющиеся имена аудио будут удалены из обучения. Убедитесь, что наборы данных, которые можно выбрать не содержат те же имена аудио в нескольких файлах ZIP-файл.

    > [!TIP]
    > С помощью наборов данных из одной говорящего является обязательным для качество результатов. Если наборы данных, которые вы отправили для обучения содержит общее число меньше, чем 6 000 уникальных фразы, необходимо обучить модель голоса с применением статистических параметрического синтеза. В случае, когда обучающих данных превышает общее число 6 000 уникальных фразы приступим процесса обучения с помощью способа объединения синтеза. Обычно эта технология объединения может привести к голосовой более естественным и более высокая точность результатов. [Обратитесь в службу пользовательские голосовые](mailto:speechsupport@microsoft.com) требуется для обучения модели с новейшей технологией нейронной TTS, может выдавать цифровой голос эквивалентно публично доступной [нейронной голоса](language-support.md#neural-voices).

5.  Нажмите кнопку **Train** чтобы начать создание модели голоса.

В таблице обучающих отображает новую запись, которая соответствует только что созданной модели. Она также отображает состояние: Обработки, выполнена успешно, сбой.

Состояние, которое отображается отражает процесс преобразования набора данных в модель голоса, как показано ниже.

| Состояние | Значение |
| ----- | ------- |
| Обработка | При создании вашей модели голоса. |
| Succeeded | Голоса модель будет создана и могут быть развернуты. |
| Сбой | Голосовые модели завершилась с ошибкой при обучении вызвано разными причинами, например невидимым проблем и неполадок в сети. |

Время обучения зависит от объема обработанных звуковых данных. Обычный диапазон времени варьируется от 30 минут для сотен выражений до 40 часов — для 20 000 выражений. После успешного выполнения обучения модели можно начать тестирование.

> [!NOTE]
> Пользователи бесплатной подписки (F0) можно обучить один голос одновременно. Стандартная подписка (S0) пользователей можно обучить три голоса одновременно. Если вы достигли предела, подождите, пока хотя бы один из ваших голосов завершит обучение, а затем повторите попытку.

> [!NOTE]
> Максимальное число голосовых моделей разрешено обучены на одну подписку — 10 моделей для пользователей бесплатной подписки (F0) до 100 пользователей подписка standard (S0).

## <a name="test-your-voice-model"></a>Тестирование модели голоса

После успешного создания голос можно протестировать перед развертыванием.

1.  Перейдите к **преобразования текста в речь > пользовательские голосовые > тестирование**.

2.  Нажмите кнопку **добавить тест**.

3.  Выберите один или несколько моделей, которые вы хотите проверить.

4.  Укажите текст, который будет voice(s) для озвучки. Если вы выбрали для тестирования нескольких моделей, за один раз, и тот же текст будет использоваться для тестирования для различных моделей.

    > [!NOTE]
    > Язык текста должен быть таким же, что и язык голоса. Можно тестировать только успешно обученных моделей. На этом шаге поддерживается только обычный текст.

5.  Нажмите кнопку **Создать**.

После отправки запроса теста, вы вернетесь на страницу тестирования. Теперь таблица содержит запись, соответствующую новому запросу, и столбец состояния. Синтезирование голоса может занять несколько минут. Если в столбце состояния говорит **Succeeded**, можно воспроизводить звук или загрузить ввод текста (txt-файл), так и аудио вывода (WAV-файла), а также дополнительно audition последний качества.

Также можно найти результаты теста на странице сведений каждого моделей, выбранных для тестирования. Перейдите к **обучения** , а щелкните имя модели, чтобы войти на страницу детализации модели.

## <a name="create-and-use-a-custom-voice-endpoint"></a>Создание и использование конечной точки настраиваемых голосовых

После того как вы успешно создали и протестировали свою голосовую модель, она развертывается в пользовательской конечной точке службы "Преобразование текста в речь". Затем эта конечная точка используется вместо обычной конечной точки при выполнении запросов службы "Преобразование текста в речь" через REST API. Пользовательской конечной точке может вызываться только подписки, который использовался для развертывания шрифта.

Чтобы создать новую конечную точку настраиваемых голосовых, перейдите к **преобразования текста в речь > пользовательские голосовые > развертывания**. Выберите **добавить конечную точку** и введите **имя** и **описание** для пользовательской конечной точке. Выберите пользовательские голосовые модели, которые вы хотите связать с этой конечной точкой.

После нажатия кнопки **добавить** кнопки, в таблице конечной точки, вы увидите запись для новой конечной точки. Создание конечной точки может занять несколько минут. Если состояние развертывания — **Удачно**, конечная точка готова к использованию.

> [!NOTE]
> Бесплатная подписка (F0) пользователи могут иметь только одну модель развертывания. Стандартная подписка (S0) пользователи могут создавать до 50 конечных точек, каждая из которых свои собственные пользовательские голосовые модели.

> [!NOTE]
> Чтобы использовать настраиваемые голоса, необходимо указать имя модели голоса, использовать настраиваемый код URI непосредственно в запрос HTTP и использовать ту же подписку, чтобы пройти проверку подлинности службы преобразования текста в РЕЧЬ.

После развертывания конечной точки, отображается имя конечной точки в виде ссылки. Щелкните ссылку, чтобы отобразить сведения, относящиеся к конечной точке, например ключа конечной точки, URL-адрес конечной точки и примеры кода.

Тестирование конечной точки в сети также доступно через портал настраиваемых пользовательских голосовых моделей. Чтобы протестировать конечную точку, выберите **проверка конечной точки** из **сведения о конечной точке** страницы. Откроется страница тестирования конечной точки. Введите текст для произнесения (в любом текстовом или [SSML формат](speech-synthesis-markup.md) в текстовом поле. Нажмите кнопку **Воспроизведение**, чтобы прослушать текст с помощью пользовательской голосовой модели. Эта функция тестирования будут осуществлять оплату по использования синтеза настраиваемое преобразование речи.

Пользовательская конечная точка функционально идентична стандартной конечной точке, используемой для запросов преобразования текста в речь. Дополнительные сведения см. в статье о [REST API](rest-text-to-speech.md).

## <a name="next-steps"></a>Дальнейшие действия

* [Руководство по: Запишите ваших примеров голоса](record-custom-voice-samples.md)
* [Справочник по API преобразования текста в речь](rest-text-to-speech.md)
