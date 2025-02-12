---
title: Управление итерациями эксперимента
titleSuffix: Azure Machine Learning Studio
description: Как управлять итерациями экспериментов в Студии машинного обучения Azure. В любой момент вы можете просмотреть данные предыдущих выполнений эксперимента, чтобы поставить под вопрос, пересмотреть и, в конечном счете, подтвердить или уточнить предыдущие предположения.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: xiaoharper
ms.author: amlstudiodocs
ms.custom: seodec18
ms.date: 03/20/2017
ms.openlocfilehash: 34a72f2e7b6be90654c0f053d5b8978b0283d56c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "60860258"
---
# <a name="manage-experiment-iterations-in-azure-machine-learning-studio"></a>Управление итерациями экспериментов в Студии машинного обучения Azure
Разработка модели прогнозной аналитики идет по итерациям — по мере изменения разных функций и параметров экспериментов выполняется сведение результатов, пока не будет получена обученная эффективная модель. Ключ к этому процессу — отслеживание различных итераций параметров и конфигураций эксперимента.



В любой момент вы можете просмотреть данные предыдущих выполнений эксперимента, чтобы поставить под вопрос, пересмотреть и, в конечном счете, подтвердить или уточнить предыдущие предположения. При запуске эксперимента Студия машинного обучения ведет журнал выполнения, включая наборы данных, модули, подключения к портам и параметры. В этот журнал также записываются результаты выполнения, данные среды выполнения, в том числе время начала и остановки, сообщения журнала и состояние выполнения. Вы можете в любое время взглянуть на эти данные, чтобы просмотреть хронологию эксперимента и промежуточные результаты. Можно даже использовать предыдущий запуск эксперимента, чтобы запустить новый этап исследования и обнаружения на пути к созданию простых, сложных или даже групповых решений моделирования.

> [!NOTE]
> При просмотре данных предыдущего выполнения эксперимента версия данного эксперимента блокируется и не может быть изменена. Тем не менее, можно сохранить копию, щелкнув **СОХРАНИТ КАК** и указав новое имя для копии. Новая копия откроется в Студии машинного обучения, и ее можно будет изменять и запускать. Эта копия вашего эксперимента доступна в списке **ЭКСПЕРИМЕНТЫ** , как и все другие ваши эксперименты.
> 
> 

## <a name="viewing-the-prior-run"></a>Просмотр предыдущего выполнения
Если у вас есть открытый эксперимент, который был выполнен хотя бы один раз, вы можете просмотреть данные предыдущего выполнения, щелкнув **Предыдущее выполнение** на панели свойств.

Для примера предположим, что вы создали эксперимент и выполнили его версии в 11:23, 11:42 и 11:55. Если вы откроете данные последнего выполнения эксперимента (11:55) и щелкнете **Предыдущее выполнение**, откроется версия, выполненная в 11:42.

## <a name="viewing-the-run-history"></a>Просмотр журнала выполнения
Вы можете просмотреть данные всех предыдущих выполнений эксперимента, щелкнув **Просмотреть журнал выполнения** в открытом эксперименте.

Для примера предположим, что вы создаете эксперимент с использованием модуля [Линейная регрессия][linear-regression] и хотите просмотреть, как изменение значения **Скорость обучения** влияет на результаты эксперимента. Вы выполняете эксперимент несколько раз с разными значениями этого параметра, как показано ниже:

| Значение скорости обучения | Время начала выполнения |
| --- | --- |
| 0,1 |11\.9.2014, 16:18:58 |
| 0,2 |11\.9.2014, 16:24:33 |
| 0,4 |11\.9.2014, 16:28:36 |
| 0,5 |11\.9.2014, 16:33:31 |

Если щелкнуть **ПРОСМОТРЕТЬ ЖУРНАЛ ВЫПОЛНЕНИЯ**, вы увидите список всех выполнений:

![Пример журнала выполнения](./media/manage-experiment-iterations/viewrunhistory.jpg)

Щелкните любое из этих выполнений, чтобы просмотреть моментальный снимок эксперимента на момент его выполнения. Сохраняется все — конфигурация, значения параметров, комментарии и результаты, чтобы предоставить вам полную запись выполнения эксперимента.

> [!TIP]
> Чтобы задокументировать итерации эксперимента, можно изменять заголовок при каждом выполнении, можно обновлять **сводку** эксперимента на панели свойств и можно добавлять или обновлять комментарии для отдельных модулей, чтобы записывать вносимые изменения. Заголовок, сводка и комментарии для модуля сохраняются при каждом выполнении эксперимента.
> 
> 

В списке экспериментов на вкладке **ЭКСПЕРИМЕНТЫ** в Студии машинного обучения всегда отображается последняя версия эксперимента. Если открыть данные предыдущего выполнения эксперимента (используя **Prior Run** (Предыдущее выполнение) или **View run history** (Просмотреть журнал выполнения)), вы сможете вернуться к черновой версии, щелкнув **View run history** (Просмотреть журнал выполнения) и выбрав итерацию, у которой в качестве значения параметра **Состояние** указано **Редактируемый**.

## <a name="iterating-on-a-previous-run"></a>Перебор итераций предыдущего выполнения
Если щелкнуть **Prior Run** (Предыдущее выполнение) или **View run history** (Просмотреть журнал выполнения) и открыть данные предыдущего выполнения, то можно просмотреть завершенный эксперимент в режиме только для чтения.

Если вы хотите начать итерацию своего эксперимента, использовав параметры, настроенные для предыдущего выполнения, это можно сделать, щелкнув **Сохранить как**. При этом создается новый эксперимент с новым заголовком и пустым журналом выполнения, все компоненты и значения параметров которого взяты из предыдущего выполнения. Этот новый эксперимент отображается на вкладке **Эксперименты** на домашней странице Студии машинного обучения. Вы можете изменить и выполнить его, начав новый журнал выполнения для этой итерации своего эксперимента. 

Для примера предположим, в предыдущем разделе есть журнал выполнения эксперимента. Вы хотите посмотреть, что происходит, если задать **скорость обучения** 0,4, и опробовать различные значения параметра **Number of training epochs** (Количество эпох обучения).

1. Щелкните **ПРОСМОТРЕТЬ ЖУРНАЛ ВЫПОЛНЕНИЯ** и откройте итерацию эксперимента, выполненную в 16:28:36 (в которой вы задали значение параметра равным 0,4).
2. Щелкните **СОХРАНИТЬ КАК**.
3. Введите новый заголовок и щелкните флажок **ОК** . Будет создана новая копия эксперимента.
4. Измените значение параметра **Количество эпох обучения** .
5. Щелкните **Выполнить**.

Теперь можно продолжить изменение и выполнение данной версии эксперимента, создавая новый журнал выполнения для записи вашей работы.

<!-- Module References -->
[linear-regression]: https://msdn.microsoft.com/library/azure/31960a6f-789b-4cf7-88d6-2e1152c0bd1a/
