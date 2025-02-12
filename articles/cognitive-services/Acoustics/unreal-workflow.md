---
title: Руководство по разработке Project Acoustics в Unreal
titlesuffix: Azure Cognitive Services
description: В этом руководстве описан рабочий процесс разработки для Project Acoustics в Unreal и Wwise.
services: cognitive-services
author: kegodin
manager: nitinme
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: tutorial
ms.date: 03/20/2019
ms.author: kegodin
ms.openlocfilehash: 1692032b093cd6189cac3ea3f63c563d9accd8ed
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/29/2019
ms.locfileid: "67477830"
---
# <a name="project-acoustics-unrealwwise-design-tutorial"></a>Руководство по разработке Project Acoustics в Unreal и Wwise
В этом руководстве описывается настройка и рабочий процесс разработки для Project Acoustics в Unreal и Wwise.

Необходимые компоненты программного обеспечения:
* проект Unreal с подключаемыми модулями Unreal и Wwise для Project Acoustics.

Для получения проекта Unreal с Project Acoustics вы можете:
* выполнить инструкции [интеграции Project Acoustics с Unreal](unreal-integration.md), чтобы добавить Project Acoustics в проект Unreal или
* использовать [пример проекта Project Acoustics](unreal-quickstart.md).

## <a name="setup-project-wide-wwise-properties"></a>Настройка свойств проекта Wwise
Wwise содержит глобальные препятствия и кривые окклюзий, которые влияют на то, как подключаемый модуль Project Acoustics управляет звуковым DSP Wwise.

### <a name="design-wwise-occlusion-curves"></a>Разработка кривых окклюзий Wwise
Когда Project Acoustics активен, он реагирует на кривые объема окклюзии, низкочастотный фильтр (LPF) и высокочастотный фильтр (HPF), которые были заданы в Wwise. Мы рекомендуем установить линейный тип кривой объема со значением –100 дБ для значения окклюзии 100.

С помощью этой настройки, если моделирование Project Acoustics вычисляет окклюзию –18 дБ, она будет вводиться в нижнюю кривую при X = 18, а к соответствующему значению Y будет применено ослабление. Чтобы выполнить половину окклюзии, установите конечную точку на –50 дБ или на –200 дБ вместо –100 дБ для увеличения окклюзии. Вы можете адаптировать и совершенствовать любую кривую, которая лучше всего подходит для вашей игры.
 
![Снимок экрана с редактором кривых окклюзий Wwise](media/wwise-occlusion-curve.png)

### <a name="disable-wwise-obstruction-curves"></a>Отключение кривых препятствий Wwise
Кривые препятствий Wwise влияют на уровень обработки в изоляции, но Project Acoustics использует элементы управления разработкой и моделирование для принудительного применения коэффициентов обработки/отсутствия обработки. Мы рекомендуем отключить кривую объема препятствий. Для разработки влажности используйте элемент управления Wetness Adjust, описанный ниже.
 
Если вы используете кривые LPF/HPF в других целях, убедитесь, что задали им значение Y = 0 при X = 0 (то есть, LPF или HPF существуют, если есть препятствие).

![Снимок экрана с редактором кривых препятствий Wwise](media/wwise-obstruction-curve.png)

### <a name="design-project-acoustics-mixer-parameters"></a>Разработка параметров микшера Project Acoustics
Вы можете управлять свойствами глобальной реверберации, перейдя на вкладку подключаемого модуля микшера шины Project Acoustics. Чтобы открыть панель параметров подключаемого модуля микшера, дважды щелкните Project Acoustics Mixer (Custom) (Project Acoustics Mixer (пользовательский)).

Вы также увидите, что подключаемый модуль микшера имеет параметр Perform Spatialization (Выполнить cпатиализацию). Если вы предпочитаете использовать встроенную спатиализацию Project Acoustics, установите флажок Perform Spatialization (Выполнить cпатиализацию) и выберите HRTF или панорамирование. Не забудьте отключить любые настроенные вами шины Dry Aux, иначе вы услышите непосредственный путь дважды. Используйте Wetness Adjust (Настройка влажности) и Reverb Time Scale Factor (Коэффициент масштабирования времени реверберации) для проверки глобальных элементов управления на смешанной реверберации. Обратите внимание, что, прежде чем начать воспроизведение, для того, чтобы получить изменения конфигурации подключаемого модуля микшера, например флажок Perform Spatialization (Выполнить cпатиализацию), необходимо перезапустить Unreal, а затем повторно создать звуковые контейнеры.

![Снимок экрана с параметрами подключаемого модуля микшера Wwise Project Acoustics](media/mixer-plugin-global-settings.png)

## <a name="set-project-acoustics-design-controls-in-the-wwise-actor-mixer-hierarchy"></a>Настройка элементов управления разработкой Project Acoustics в иерархии субъект-микшер Wwise
Чтобы управлять параметрами отдельного субъекта-микшера, дважды щелкните Actor-Mixer, а затем выберите вкладку Mixer Plug-in (Подключаемый модуль микшера). Здесь вы сможете изменить любые параметры на уровне каждого звука. Эти значения объединяются с набором значений со стороны Unreal (описано ниже). Например, если подключаемый модуль Unreal Project Acoustics присваивает Outdoorness Adjustment (Корректировка открытого пространства) на объекте значение 0,5, а Wwise устанавливает ему значение –0,25, в результате корректировка, присвоенная этому звуку, получает значение 0,25.

![Снимок экрана с параметрами микшера для каждого звука в иерархии субъекта-микшера Wwise](media/per-sound-mixer-settings.png)

### <a name="ensure-the-aux-bus-has-dry-send-and-output-bus-has-wet-send"></a>Проверка содержания шиной aux необработанной отправки, а шиной выходных данных — отправки с преобладанием обработки
Помните, что требуемая установка субъекта-микшера обменивает обычные обработанные и необработанные маршруты в Wwise. Она выдает сигнал реверберации на шине выходных данных субъекта-микшера (настроенной на шину Project Acoustics) и необработанный сигнал по шине aux, определяемой пользователем. Такая маршрутизация необходима из-за особенностей API подключаемого модуля микшера Wwise, которое используется подключаемым модулем Wwise Project Acoustics.

![Снимок экрана с редактором Wwise с рекомендациями по проектированию голоса для Project Acoustics](media/voice-design-guidelines.png)
 
### <a name="set-up-distance-attenuation-curves"></a>Настройка кривых затухания на основе расстояния
Убедитесь, что любая кривая затухания, используемая несколькими субъектами-микшерами с помощью Project Acoustics, содержит набор отправок aux, определяемых пользователем, настроенных на значение объема шины выходных данных. По умолчанию Wwise делает это для только что созданных кривых затухания. При переносе существующего проекта, проверьте параметры кривой.

По умолчанию моделирование Project Acoustics имеет радиус 45 метров от расположения проигрывателя. Обычно на этом расстоянии мы рекомендуем устанавливать кривую затухания на –200 дБ. Это расстояние не является жестким ограничением. Для некоторых звуков, например для звука оружия, можно установить немного больший радиус. Однако в таком случае следует помнить, что учитываться будет только геометрия в пределах 45 м от расположения проигрывателя. Если проигрыватель расположен в комнате, а источник звука находится за пределами помещения на расстоянии 100 м, он будет полностью перекрыт. Если источник звука расположен в комнате, а проигрыватель находится за пределами помещения на расстоянии 100 м, он не будет перекрыт.

![Снимок экрана с кривыми затухания в Wwise](media/atten-curve.png)

### <a name="post-mixer-equalization"></a>Постмикшерная частотная коррекция ###
 Вы также можете добавить постмикшерный эквалайзер. Шину Project Acoustics можно считать типичной шиной реверберации (в режиме реверберации по умолчанию) и настроить для нее фильтр частотной коррекции. Пример можно найти в примере проекта Wwise Project Acoustics.

![Снимок экрана постмикшерного эквалайзера Wwise](media/wwise-post-mixer-eq.png)

Например, фильтр верхних частот помогает обработать басы записей в ближнем поле с гулкой нереалистичной реверберацией. Кроме того, можно добиться большего контроля после акустического моделирования, регулируя эквалайзер с помощью элементов управления параметрами в реальном времени, что позволяет изменять цвет реверберации во время выполнения.

## <a name="set-up-scene-wide-project-acoustics-properties"></a>Настройка свойств Project Acoustics на уровне сцены

Субъект акустического пространства предоставляет много элементов управления, которые изменяют реакцию системы и помогают при отладке.

![Снимок экрана с элементами управления акустическим пространством Unreal](media/acoustics-space-controls.png)

* **Acoustics Data** (акустические данные). Этому полю необходимо присвоить ресурс акустического моделирования из каталога Content/Acoustics. Подключаемый модуль Project Acoustics автоматически добавит Content/Acoustics в запакованные каталоги проекта.
* **Tile size** (Размер плитки). Экстенты области вокруг прослушивателя, акустические данные из которой необходимо загружать в ОЗУ. Пока пробы прослушивателя в непосредственной близости от проигрывателя загружаются, результаты будут такими же как и при загрузке акустических данных для всех проб. Чем больше плитки, тем больший объем ОЗУ используется, но тем меньший дисковый ввод-вывод требуется.
* **Auto Stream** (Автоматический поток). Если этот параметр включен, новые плитки автоматически загружаются, когда прослушиватель достигает края загруженной области. Если он отключен, вам потребуется загружать новые плитки вручную с помощью кода или схем.
* **Cache Scale** (Масштабирование кэша). Управляет размером кэша, используемым для акустических запросов. Для кэша меньшего размера необходимо меньшее количество ОЗУ, но потребление ЦП для каждого запроса может увеличиться.
* **Acoustics Enabled** (Включенная акустика). Элемент управления отладкой для включения быстрого переключения A/B акустического моделирования. Этот элемент управления не учитывается в конфигурациях доставки. Этот элемент управления позволяет определить, возникла ли конкретная звуковая ошибка в вычислениях акустики или какая-либо другая проблема в проекте Wwise.
* **Update Distances** (Обновление расстояния). Этот параметр позволяет вам использовать сведения предварительного акустического моделирования для запросов расстояния. Эти запросы подобны "лучевым бликам", но они были предварительно вычислены и соответственно поглощают гораздо меньше ресурсов ЦП. Примером использования являются дискретные отражения от ближайшей поверхности к прослушивателю. Чтобы использовать этот параметр в полной мере, для запроса расстояния необходимо использовать код или схемы.
* **Draw Stats** (Отрисовка статистики). Хотя в UE `stat Acoustics` может предоставить вам информацию о ЦП, это отображение состояния покажет загруженный ACE-файл, потребление ОЗУ и другие сведения о состоянии в верхней левой части экрана.
* **Draw Voxels** (Отрисовка объемных пикселей). Накладывает объемные пиксели близко к прослушивателю с отображением сетки объемных пикселей, используемой во время интерполяции среды выполнения. Если передатчик находится внутри объемных пикселей среды выполнения, то он приведет к завершению акустических запросов сбоем.
* **Draw Probes** (Отрисовка проб). Отображает все пробы этой сцены. В зависимости от состояния загрузки они будут отличаться цветами.
* **Draw Distances** (Отрисовка расстояний). Если свойство Update Distances (Обновление расстояния) включено, на ближайшей к прослушивателю поверхности в квантованных направлениях вокруг прослушивателя отобразится поле.

## <a name="actor-specific-acoustics-design-controls"></a>Элементы управления разработкой конкретного субъекта акустики
Эти элементы управления разработкой действуют на отдельном компоненте аудио в Unreal.

![Снимок экрана с элементами управления компонентами аудио Unreal](media/audio-component-controls.png)

* **Occlusion Multiplier** (Множитель окклюзии). Определяет эффект окклюзии. Значения больше 1 повысят окклюзию. Значения меньше 1 сведут окклюзию к минимуму.
* **Wetness Adjustment** (Корректировка обработки). Дополнительные дБ реверберации.
* **Decay Time Multiplier** (Множитель времени затухания). Управляет RT60 множительно на основе выходных данных акустического моделирования.
* **Outdoorness Adjustment** (Корректировка открытого пространства). Определяет, какой является реверберация открытого пространства. Значения ближе к 0 применяются внутри помещения, а ближе к 1 — на открытом пространстве. Эта корректировка является адаптивной, следовательно значение –1 усилит реверберацию внутри помещения, а +1 — на открытом пространстве.
* **Transmission Db** (Передача данных в дБ). Воспроизводит дополнительный звук "сквозь стену" с уровнем громкости в сочетании с линей видимости на основе затухания расстояния.
* **Wet Ratio Distance Warp** (Искажение расстояния коэффициента обработки). Корректирует характеристики реверберации в источнике таким образом, чтобы он находился ближе или дальше, при этом не затрагивая прямой путь.
* **Play on Start** (Воспроизведение при запуске). Установите этот параметр, чтобы указать, будет ли звук воспроизводиться автоматически в начале сцены. Включен по умолчанию.
* **Show Acoustic Parameters** (Показать параметры акустики). Отображает сведения об отладке непосредственно поверх внутриигрового компонента. (только для конфигураций, отсутствующих в доставке)

## <a name="blueprint-functionality"></a>Функциональные возможности схемы
Доступ к субъекту акустического пространства можно получить через схему, обеспечив такие функции, как загрузка карты или изменение параметров с помощью уровня написания скриптов. Здесь мы приводим два примера.

### <a name="add-finer-grained-control-over-streaming-load"></a>Добавление более точного контроля над потоковой передачей загрузки
Чтобы управлять потоковой передачей данных акустики самостоятельно вместо автоматической потоковой передачи, основанной на положении проигрывателя, вы можете использовать функцию схемы Force Load Tile (Элемент принудительной загрузки).

![Снимок экрана с параметрами потоковой передачи схемы в Unreal](media/blueprint-streaming.png)

* **Target** (Цель). Субъект AcousticsSpace.
* **Center Position** (Центральная позиция). Центр области, в которую необходимо загрузить данные.
* **Unload Probes Outside Tile** (Выгрузка проб снаружи плитки). Если установлен этот флажок, все пробы, которые находятся вне новой области, будут выгружены из ОЗУ. Если флажок снят, новая область загружается в память, оставляя существующие пробы загруженными в память.
* **Block on Completion** (Блокировка по завершению). Преобразует загрузку в синхронную операцию.

Размер элементов необходимо установить прежде, чем вызывать Force Load Tile (Элемент принудительной загрузки). Например, вы можете сделать что-то подобное, чтобы загрузить файл ACE, установить размер плитки и выполнить потоковую передачу в области.

![Снимок экрана с параметрами потоковой передачи в Unreal](media/streaming-setup.png)

Функция схемы загрузки данных акустики, используемая в этом примере, имеет следующие параметры:

* **Target** (Цель). Субъект AcousticsSpace.
* **New Bake** (Создать модель). Ресурс данных акустики, который необходимо загрузить. Если оставить этот параметр пустым или задать для него значение null, текущая модель будет выгружена без загрузки новой.

### <a name="optionally-query-for-surface-proximity"></a>Дополнительный запрос для поверхности с учетом расположения
Вы можете использовать функцию Query Distance (Запросить расстояние), чтобы увидеть, насколько близко находятся поверхности в определенном направлении вокруг прослушивателя. Эта функция позволяет управлять направленными отложенными отражениями, а также используется для другой логики игры, обусловленной поверхностью с учетом расположения. Запрос дешевле, чем лучевые блики, так как результаты извлекаются из таблицы подстановки акустики.

![Снимок экрана с примером запроса расстояния схемы](media/distance-query.png)

* **Target** (Цель). Субъект AcousticsSpace.
* **Look Direction** (Поиск направления). Направление для запроса с центром в прослушивателе.
* **Distance** (Расстояние). Если запрос выполнен успешно, указывается расстояние до ближайшей поверхности.
* **Return Value** (Возвращаемое значение). Логическое значение; равно true, если запрос выполнен успешно, в противном случае — false.

## <a name="next-steps"></a>Дополнительная информация
* Продолжите изучение принципов [процесса разработки](design-process.md).
* [Создайте учетную запись Azure](create-azure-account.md) для моделирования собственной сцены.


