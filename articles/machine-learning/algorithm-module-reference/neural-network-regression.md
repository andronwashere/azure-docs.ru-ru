---
title: 'Регрессия нейронной сети: Ссылка на модуль'
titleSuffix: Azure Machine Learning service
description: Узнайте, как использовать модуль регрессии нейронной сети в службе машинного обучения Azure для создания модели регрессии с помощью настраиваемого алгоритма нейронной сети...
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ROBOTS: NOINDEX
ms.openlocfilehash: bc6a7505ab09e929e5add61eea687f871aedf242
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "65029315"
---
# <a name="neural-network-regression-module"></a>Модуль нейронной сети регрессии

*Создает модель регрессии с помощью алгоритма нейронной сети*  
  
 Категория: Машинное обучение / модель инициализации / Регрессия
  
## <a name="module-overview"></a>Общие сведения о модуле  

В этой статье описывается модуль визуального интерфейса (Предварительная версия) для службы машинного обучения Azure.

Этот модуль используется для создания модели регрессии с помощью настраиваемого алгоритма нейронной сети.
  
 Несмотря на то, что нейронные сети широко используются для глубокого обучения и моделирования сложных задач, таких как распознавание изображений, они легко адаптируются к задачам регрессии. Любой класс статистических моделей можно назвать нейронной сетью, если они используют адаптивные весовые коэффициенты и можно приблизительно нелинейных функций входных данных. Таким образом Регрессия нейронной сети подходит для задач, где более традиционных моделей нельзя решить с.
  
 Регрессия нейронной сети — это метод контролируемого обучения и поэтому требует *с тегами набора данных*, который включает в себя столбец метки. Так как модель регрессии прогнозирует числовое значение, столбца "label" должен быть типом числовых данных.  
  
 Вы можете обучить модель, предоставляя модели и маркированному набору данных в качестве входных данных [Обучение модели](./train-model.md). Затем обученную модель можно использовать для прогнозирования новых примеров входных данных.  
  
## <a name="configure-neural-network-regression"></a>Настройка регрессии нейронной сети 

Нейронных сетей очень гибко настраивается. В этом разделе описывается создание модели с помощью двух методов:
  
+ [Создать модель нейронной сети, с помощью архитектуры по умолчанию](#bkmk_DefaultArchitecture)  
  
    Если вы принимаете в архитектуре нейронной сети по умолчанию, используйте **свойства** панели, чтобы задать параметры, управляющие поведением нейронной сети, такие как количество узлов в скрытом слое, скорость обучения и нормализации.

    Начните отсюда, если вы не знакомы с нейронными сетями. Модуль поддерживает множество настроек, а также настройку модели без глубоких знаний в области нейронных сетей. 

+ Определить индивидуальную архитектуру для нейронной сети 

    Используйте этот параметр, если вы хотите добавить дополнительные скрытые слои или полностью настроить сетевые архитектуры, подключения и активации функции.
    
    Этот вариант подойдет, если вы уже знакомы с помощью нейронных сетей. Используйте язык Net # для определения архитектуры сети.  

##  <a name="bkmk_DefaultArchitecture"></a> Создать модель нейронной сети, с помощью архитектуры по умолчанию
  
1.  Добавить **регрессии нейронной сети** эксперимент в интерфейсе модуль. Этот модуль, в разделе можно найти **машинного обучения**, **инициализировать**в **регрессии** категории. 
  
2. Укажите способ модели для обучения, задав **Создание режима учителя** параметр.  
  
    -   **Единый параметр**: Выберите этот параметр, если вы уже знаете, как вы хотите настроить модель.  

3.  В **скрытых слоев спецификации**выберите **полностью подключен случай**. Этот параметр создает модель, используя в архитектуре нейронной сети по умолчанию, имеющей для модели регрессии нейронной сети, эти атрибуты:  
  
    + Сети содержит ровно один скрытый слой.
    + Выходной слой полностью подключен к скрытому слою и скрытый слой полностью подключен ко входному слою.
    + Количество узлов в скрытом слое, задаваемых пользователем (значение по умолчанию — 100).  
  
    Поскольку количество узлов во входном слое определяется количеством признаков в обучающих данных, в модели регрессии может существовать только один узел на выходном слое.  
  
4. Для **количество скрытых узлов**, введите количество скрытых узлов. По умолчанию используется один скрытый слой с 100 узлов. (Этот параметр недоступен, если определить индивидуальную архитектуру, используя язык Net #.)
  
5.  Для **скорость обучения**, введите значение, которое определяет шаг, выполненный на каждой итерации, до осуществления коррекции. Увеличьте значение скорости обучения может привести к ускорить Сходимость модели, но возможны Промахи на локальных минимумах.

6.  Для **число итераций обучения**, задать максимальное количество повторов, алгоритм обрабатывает обучающих вариантов.

7.  Для ** начальных весовых коэффициентов обучения диаметру, введите значение, определяющее весовые коэффициенты узлов в начале процесса обучения.

8.  Для **импульс**, введите значение для применения во время обучения как вес на узлах из предыдущих итераций.

10. Выберите параметр **случайный порядок примеров**, чтобы изменить порядок обращений между итерациями. Если вы отключите этот параметр, случаи обрабатываются в точно в том порядке каждый раз при запуске эксперимента.
  
11. Для **начальное значение случайного числа**, при необходимости можно ввести значение для использования в качестве начального значения. Указание начального значения полезно при необходимо проверить повторяемость несколькими запусками одного эксперимента.
  
13. Подключите обучающий набор данных и один из [обучающих модулей](module-reference.md): 
  
    -   Если задать **Создание режима учителя** для **один параметр**, использовать [Обучение модели](./train-model.md).  
  
   
14. Запустите эксперимент.  

## <a name="results"></a>Результаты

После завершения обучения:

+ Чтобы просмотреть сводку параметров модели, а также функцию весовые коэффициенты, узнали из обучения и другие параметры нейронной сети, щелкните правой кнопкой мыши выходные данные [Обучение модели](./train-model.md)и выберите **визуализировать**.  

+ Чтобы сохранить моментальный снимок обученной модели, щелкните правой кнопкой мыши **модели Trained** выходных данных и выберите **Сохранить как Обученную модель**. Эта модель не обновляется при последующих запусках одного эксперимента.


## <a name="next-steps"></a>Дальнейшие действия

См. в разделе [набор модулей, доступных](module-reference.md) для службы машинного обучения Azure. 